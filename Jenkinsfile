pipeline {
    agent any

    parameters {
        string(name: 'USERNAME', defaultValue: 'Roman', description: 'Enter your name')
        string(name: 'AGE', defaultValue: '22', description: 'Enter your age')
    }

    stages {
        stage('Start') {
            steps {
                echo 'Lab_2: started by GitHub'
            }
        }

        stage('Image build') {
            steps {
                sh "docker build -t prikm:latest ."
                sh "docker tag prikm romanmitpa2024/prikm:latest"
                sh "docker tag prikm romanmitpa2024/prikm:$BUILD_NUMBER"
            }
        }

        stage('Push to registry') {
            steps {
                withDockerRegistry([credentialsId: "dockerhub_token", url: ""]) {
                    sh "docker push romanmitpa2024/prikm:latest"
                    sh "docker push romanmitpa2024/prikm:$BUILD_NUMBER"
                }
            }
        }

        stage('Push artifact v1') {
            steps {
                withDockerRegistry([credentialsId: "dockerhub_token", url: ""]) {
                    sh "docker tag prikm romanmitpa2024/prikm:v1"
                    sh "docker push romanmitpa2024/prikm:v1"
                }
            }
        }

        stage('Push artifact v2') {
            steps {
                withDockerRegistry([credentialsId: "dockerhub_token", url: ""]) {
                    sh "docker tag prikm romanmitpa2024/prikm:v2"
                    sh "docker push romanmitpa2024/prikm:v2"
                }
            }
        }

        stage('Deploy image') {
            steps {
                sh "docker run -d -p 80:80 romanmitpa2024/prikm"
            }
        }

        stage('Hello') {
            steps {
                script {
                    def msg = "Hello, ${params.USERNAME}!"
                    echo msg
                    slackNotify(msg)
                }
            }
        }

        stage('Greeting') {
            steps {
                script {
                    def msg = "Hello again, ${params.USERNAME}!"
                    echo msg
                    slackNotify(msg)
                }
            }
        }

        stage('Write File') {
            steps {
                script {
                    def msg = "Welcome, ${params.USERNAME}"
                    writeFile file: 'message.txt', text: msg
                    echo 'File written.'
                    slackNotify("File written with message: ${msg}")
                }
            }
        }

        stage('Read File') {
            steps {
                script {
                    def content = readFile 'message.txt'
                    echo "File contains: ${content}"
                    slackNotify("File contains: ${content}")
                }
            }
        }

        stage('Slack Notify User Info') {
            steps {
                script {
                    slackNotify("User: ${params.USERNAME}\nAge: ${params.AGE}")
                }
            }
        }
    }
}

def slackNotify(String message) {
    withCredentials([string(credentialsId: 'slack-webhook-url', variable: 'SLACK_WEBHOOK')]) {
        sh """
            curl -X POST -H 'Content-type: application/json' \
            --data '{"text": "${message}"}' \
            $SLACK_WEBHOOK
        """
    }
}













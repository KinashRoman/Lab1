pipeline {
    agent any

    parameters {
        string(name: 'USERNAME', defaultValue: 'student', description: 'Enter your name')
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
                echo "Hello, ${params.USERNAME}!"
            }
        }

        stage('Greeting') {
            steps {
                echo "Hello, ${params.USERNAME}!"
            }
        }

        stage('Write File') {
            steps {
                writeFile file: 'message.txt', text: "Welcome, ${params.USERNAME}"
                echo 'File written.'
            }
        }

        stage('Read File') {
            steps {
                script {
                    def content = readFile 'message.txt'
                    echo "File contains: ${content}"
                }
            }
        }
    }

    post {
        success {
            office365ConnectorWebhooks {
                webhooks {
                    webhook {
                        name('Teams-O365')
                        url('Enter_your_URL_here') // Введіть ваш URL вебхука
                        startNotification(false)
                        notifySuccess(true)
                        notifyAborted(false)
                        notifyNotBuilt(false)
                        notifyUnstable(true)
                        notifyFailure(true)
                        notifyBackToNormal(true)
                        notifyRepeatedFailure(false)
                        timeout(30000)
                    }
                }
            }
        }
    }
}





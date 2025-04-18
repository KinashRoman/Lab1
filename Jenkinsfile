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
        always {
            script {
                def payload = """
                {
                  "text": "Jenkins: Pipeline завершено для користувача ${params.USERNAME}. Статус: ${currentBuild.currentResult}"
                }
                """
                httpRequest httpMode: 'POST',
                            contentType: 'APPLICATION_JSON',
                            requestBody: payload,
                            url: 'https://prod-25.westeurope.logic.azure.com:443/workflows/22d6d9e5e13f4f10a6eb0dbeeac14b0f/triggers/manual/paths/invoke?api-version=2016-06-01&sp=%2Ftriggers%2Fmanual%2Frun&sv=1.0&sig=brPprl_aD3YDY39h-vyqVz2q0OYTR_6fvxF6dgDh-XA'
            }
        }
    }
}







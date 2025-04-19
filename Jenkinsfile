pipeline {
    agent any

    parameters {
        string(name: 'USERNAME', defaultValue: 'student', description: 'Enter your name')
    }

    environment {
        DOCKER_IMAGE = 'prikm'
        DOCKER_TAG_LATEST = 'latest'
        DOCKER_REGISTRY = 'romanmitpa2024'
        SLACK_WEBHOOK_URL = 'https://hooks.slack.com/services/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'
    }

    stages {
        stage('Start') {
            steps {
                echo 'Lab_2: started by GitHub'
            }
        }

        stage('Image build') {
            steps {
                script {
                    echo 'Building Docker image...'
                    sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG_LATEST} ."
                    sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG_LATEST} ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${DOCKER_TAG_LATEST}"
                    sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG_LATEST} ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${BUILD_NUMBER}"
                }
            }
        }

        stage('Push to registry') {
            steps {
                withDockerRegistry([credentialsId: "dockerhub_token", url: ""]) {
                    script {
                        echo 'Pushing Docker image to registry...'
                        sh "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${DOCKER_TAG_LATEST}"
                        sh "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${BUILD_NUMBER}"
                    }
                }
            }
        }

        stage('Push artifact v1') {
            steps {
                withDockerRegistry([credentialsId: "dockerhub_token", url: ""]) {
                    script {
                        echo 'Pushing version v1 to registry...'
                        sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG_LATEST} ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:v1"
                        sh "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:v1"
                    }
                }
            }
        }

        stage('Push artifact v2') {
            steps {
                withDockerRegistry([credentialsId: "dockerhub_token", url: ""]) {
                    script {
                        echo 'Pushing version v2 to registry...'
                        sh "docker tag ${DOCKER_IMAGE}:${DOCKER_TAG_LATEST} ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:v2"
                        sh "docker push ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:v2"
                    }
                }
            }
        }

        stage('Deploy image') {
            steps {
                script {
                    echo 'Deploying Docker image...'
                    sh "docker run -d -p 80:80 ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${DOCKER_TAG_LATEST}"
                }
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
                script {
                    def message = "Welcome, ${params.USERNAME}"
                    writeFile file: 'message.txt', text: message
                    echo 'File written.'
                }
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

        stage('Notify Slack') {
            steps {
                script {
                    def message = "Build #${env.BUILD_NUMBER} finished for ${params.USERNAME}"
                    echo "Notifying Slack..."
                    sh """
                        curl -X POST -H 'Content-type: application/json' --data '{"text":"${message}"}' ${SLACK_WEBHOOK_URL}
                    """
                }
            }
        }
    }
}











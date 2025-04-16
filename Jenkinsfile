pipeline {
   agent any

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
            withDockerRegistry([ credentialsId: "dockerhub_token", url: "" ]) {
               sh "docker push romanmitpa2024/prikm:latest"
               sh "docker push romanmitpa2024/prikm:$BUILD_NUMBER"
            }
         }
      }

      stage('Push artifact v1') {
         steps {
            sh "docker tag prikm romanmitpa2024/prikm:v1"
            sh "docker push romanmitpa2024/prikm:v1"
         }
      }

      stage('Push artifact v2') {
         steps {
            sh "docker tag prikm romanmitpa2024/prikm:v2"
            sh "docker push romanmitpa2024/prikm:v2"
         }
      }

      stage('Deploy image') {
         steps {
            sh "docker run -d -p 80:80 romanmitpa2024/prikm"
         }
      }
   }
}


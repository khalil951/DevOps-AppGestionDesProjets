pipeline {
    agent any
    stages {
        // ... your existing Checkout / Build Backend / Test Backend / Build Frontend stages stay as they are ...

        stage('Build Docker Images') {
          steps {
            withCredentials([usernamePassword(
                credentialsId: 'dockerhub-credentials',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS')]) {
              sh 'docker compose build'
            }
          }
        }

        stage('Push to Docker Hub') {
          steps {
            withCredentials([usernamePassword(
                credentialsId: 'dockerhub-credentials',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASS')]) {
              sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
              sh 'docker compose push backend frontend'
            }
          }
        }

        stage('Deploy') {
          steps {
            sh 'docker compose up -d'
          }
        }
    }
}

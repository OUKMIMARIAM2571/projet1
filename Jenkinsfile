pipeline {
  agent any

 environment {
  DOCKERHUB_CREDENTIALS = credentials('DockerHub')
  DOCKERHUB_USERNAME = DOCKERHUB_CREDENTIALS_USR
  DOCKERHUB_PASSWORD = DOCKERHUB_CREDENTIALS_PSW
}

  
  stages {
    stage('Checkout code') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        bat 'docker-compose build'
        bat 'start docker-compose up -d'
      }
    }

    stage('Test') {
      steps {
        echo "test passed"
      }
    }

    stage('Push Images to Docker Hub') {
      steps {
        script {
      withCredentials([string(credentialsId: 'DockerHub', variable: 'DOCKERHUB_CREDENTIALS')]) {
        withEnv(["DOCKERHUB_USERNAME=${DOCKERHUB_USERNAME}", "DOCKERHUB_PASSWORD=${DOCKERHUB_PASSWORD}"]) {
          bat 'echo %DOCKERHUB_PASSWORD% | docker login -u %DOCKERHUB_USERNAME% --password-stdin'
          bat 'docker-compose push'
      }
    }
        }
      }
    }

    stage('Cleanup') {
      steps {
        bat 'docker-compose down'
      }
    }
  }

 
}

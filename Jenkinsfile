pipeline {
  agent any

  environment {
    IMAGE_NAME = "mrrobotthecoder/jenkins-demo"
  }

  stages {
    stage ('Clone Repository') {
      steps {
        checkout scm
      }
    }

  stage ('Build Docker Image') {
    steps {
      script {
        sh """
            docker build -t ${IMAGE_NAME}:latest .
          """
      }
    }
  }
  stage ('Push to Docker Hub') {
    steps {
      script {
          docker.withRegistry('', 'dockerhub') {
            sh "docker push ${IMAGE_NAME}:latest"
          }
        }
      }
    }
  }
}  

pipeline {
  agent any

  environment {
    IMAGE_NAME = "mrrobotthe coder/jenkins-demo"
  }

  stages {
    stage ('Clone Repository') {
      steps {
        checkout scm
      }
    }

  stage ('Build Docker Image') {
    steps {
      scripts {
        sh """
            docker build -t ${IMAGE_NAME}:latest .
          """
      }
    }
  }
  stage ('Push to Docker Hub') {
    steps {
      scripts {
          docker.withRegistry('', 'dockerhub') {
            sh """
                docker push ${IMAGE_NAME}:latest
            """
          }
        }
      }
    }
  }
}  

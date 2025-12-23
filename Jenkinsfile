pipeline {
  agent any

  environment {
    IMAGE_NAME = "mrrobotthecoder/jenkins-demo"
    KUBECONFIG = "/var/lib/jenkins/.kube/config"
  }

  stages {
    stage ('Clone Repository') {
      steps {
        checkout scm
      }
    }

  stage ('Build Docker Image') {
    steps {
      sh "docker build -t ${IMAGE_NAME}:latest . "
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
  
  stage('Deploy to Kubernetes') {
    steps {
      sh """
        kubectl apply -f k8s/
        kubectl roolout status deployment/jenkins-demo -n demo
      """
      }
    }
  }  
}

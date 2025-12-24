pipeline {
  agent any

  environment {
    IMAGE_NAME = "mrrobotthecoder/jenkins-demo"
    KUBECONFIG = "/var/lib/jenkins/.kube/config"
  }

  stages {
    stage ('Checkout') {
      steps {
        checkout scm
      }
    }

  stage ('Determine Version') {
    steps {
      script {
        if (!params.RELEASE_TAG) {
          error("RELEASE_TAG parameter is required")
        }
        env.VERSION = params.RELEASE_TAG.replaceFirst('v', '')
        echo "Building version: ${env.VERSION}"
      }
    }
  }
    
  stage ('Build Docker Image') {
    steps {
      sh "docker build -t ${IMAGE_NAME}:${VERSION} . "
    }
  }
    
  stage ('Push to Docker Hub') {
    steps {
      script {
          docker.withRegistry('', 'dockerhub') {
            sh "docker push ${IMAGE_NAME}:${VERSION}"
          }
        }
      }
    }
  
  stage('Deploy to Kubernetes') {
    steps {
      sh """
        sed -i 's/IMAGE_TAG/${VERSION}/g' k8s/deployment.yaml
        kubectl apply -f k8s/
        kubectl rollout status deployment/jenkins-demo -n demo
      """
      }
    }
  }  
}

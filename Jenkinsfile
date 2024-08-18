pipeline {
  environment {
    dockerimagename = "146587/nginx"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/AlexanderSchelokov/nginx.git'
      }
    }
    stage('Build image') {
      steps {
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image') {
      environment {
        registryCredential = 'dockerhub'
      }
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
            dockerImage.push("latest")
          }
        }
      }
    }
    stage('Update Kubernetes Deployment') {
      steps {
        script {
          sh 'sudo -S kubectl rollout restart deployment nginx-static'
        }
      }
    }
  }
}

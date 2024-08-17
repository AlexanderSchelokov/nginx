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
    stage('Checkout tag') {
      steps{
        script {
          sh 'git fetch'
          gitTag=sh(returnStdout:  true, script: "git tag --sort=-creatordate | head -n 1").trim()
          echo "gitTag output: ${gitTag}"
        }
      }
    }
    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build dockerimagename
        }
      }
    }
    stage('Pushing Image:tags') {
      environment {
               registryCredential = 'dockerhub-credentials'
           }
      steps{
        script {
          docker.withRegistry( 'https://index.docker.io/146587/', registryCredential ) {
            dockerImage.push("${gitTag}")
          }
        }
      }
    }
    stage('Restart Kubernetes Cluster') {
  steps {
    script {
      sh 'kubectl rollout restart deployment/default -n'
    }
  }
}
}

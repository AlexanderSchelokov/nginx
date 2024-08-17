pipeline {
    agent any
    environment {
        dockerimagename = "146587/nginx"
        registryCredential = credentials("dockerhub-credentials2")
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build dockerimagename
                    docker.withRegistry('https://index.docker.io/146587/', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Update Kubernetes Deployment') {
            steps {
                script {
                    sh 'kubectl set image deployment/default nginx=146587/nginx'
                }
            }
        }
    }
}

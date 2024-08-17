pipeline {
    agent any
    environment {
        dockerimagename = "146587/nginx"
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
                }
            }
        }
        stage('Pushing Image:tags') {
            environment {
                registryCredential = 'dockerhub-credentials'
            }
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/146587/', registryCredential) {
                        dockerImage.push("${gitTag}")
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

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
            steps {
                script {
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

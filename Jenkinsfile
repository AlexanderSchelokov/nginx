pipeline {
    environment {
        DOCKER_USERNAME = credentials("146587")
        DOCKER_PASSWORD = credentials("As146587+3")
    }
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD"
                    def dockerImage = docker.build('146587/nginx', '.')
                    dockerImage.push()
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

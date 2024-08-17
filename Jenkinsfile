pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
    stage('Check Image Details') {
        steps {
            script {
                def imageDetails = docker.inspect('146587/nginx')
                echo "Image ID: ${imageDetails.Id}"
                echo "Image Created: ${imageDetails.Created}"
        }
    }
}

        stage('Build Docker Image') {
            steps {
                script {
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

pipeline {
    agent { dockerfile true }
    stages {
        stage('Initialize') {
            steps {
                script {
                def dockerHome = tool 'myDocker'
                withEnv(["PATH+DOCKER=${dockerHome}/bin"]) {
                    sh 'echo $PATH'
                }
            }
        }
        stage('Checkout') {
            steps {
                checkout scm
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

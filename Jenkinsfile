pipeline {
    agent { dockerfile true }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Initialize'){
        def dockerHome = tool 'myDocker'
        env.PATH = "${dockerHome}/bin:${env.PATH}"
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

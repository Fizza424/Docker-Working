pipeline {
    agent any

    environment {
        IMAGE_NAME = 'docker-working'
        TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Fizza424/Docker-Working.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:${TAG}")
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    docker.image("${IMAGE_NAME}:${TAG}").run()
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and container run successful!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}



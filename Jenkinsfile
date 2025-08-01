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
                dir("${env.WORKSPACE}") {
                    script {
                        bat "docker build -t ${IMAGE_NAME}:${TAG} ."
                    }
                }
            }
        }

        stage('Run Container') {
            steps {
                dir("${env.WORKSPACE}") {
                    script {
                        bat "docker run --rm ${IMAGE_NAME}:${TAG}"
                    }
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



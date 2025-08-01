pipeline {
    agent any

    environment {
        IMAGE_NAME = 'fizza424/docker-working'
        CONTAINER_NAME = 'docker-working-app'
    }

    stages {
        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Run Docker Container') {
            steps {
                bat 'docker rm -f %CONTAINER_NAME% || echo "No container to remove"'
                bat 'docker run -d -p 8080:80 --name %CONTAINER_NAME% %IMAGE_NAME%'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DOCKERHUB_CREDENTIALS', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    bat 'docker login -u %DOCKER_USER% -p %DOCKER_PASS%'
                    bat 'docker push %IMAGE_NAME%'
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed!'
        }
    }
}


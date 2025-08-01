pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')
        IMAGE_NAME = 'fizza424/docker-working'
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/Fizza424/Docker-Working.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker Image...'
                bat 'docker build -t %IMAGE_NAME% .'
            }
        }

        stage('Run Container for Test') {
            steps {
                echo 'Running Container for Testing...'
                bat 'docker run -d --name test-container -p 80:80 %IMAGE_NAME%'
                bat 'timeout /t 10'
                bat 'docker stop test-container'
                bat 'docker rm test-container'
            }
        }

        stage('Push to DockerHub') {
            steps {
                echo 'Pushing Image to DockerHub...'
                bat 'docker login -u %DOCKERHUB_CREDENTIALS_USR% -p %DOCKERHUB_CREDENTIALS_PSW%'
                bat 'docker push %IMAGE_NAME%'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deployment step (customize this as needed)...'
            }
        }
    }

    post {
        failure {
            echo '❌ Pipeline failed!'
        }
        success {
            echo '✅ Pipeline succeeded!'
        }
    }
}



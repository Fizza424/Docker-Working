pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "hello-flask:${env.BUILD_ID}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Fizza424/Docker-Working.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker-compose build'
            }
        }
        
        stage('Run Unit Tests') {
            // If you have a tests folder and want to run unit tests inside the container
            steps {
                sh "docker-compose run --rm web python -m unittest discover -s tests"
            }
        }

        stage('Deploy Application') {
            steps {
                // Stop and remove any running instance
                sh 'docker-compose down'
                // Start the application in detached mode
                sh 'docker-compose up -d'
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}


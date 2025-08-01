pipeline {
    agent any

    parameters {
        string(name: 'DOCKER_IMAGE_NAME', defaultValue: 'fizza424/docker-working', description: 'Docker image name')
        string(name: 'CONTAINER_PORT', defaultValue: '5000', description: 'Port for the container')
    }

    environment {
        DOCKER_TAG = 'latest'
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds') // This must match your Jenkins credentials ID
    }

    stages {
        stage('Build Docker Image') {
            steps {
                sh """
                docker build -t ${params.DOCKER_IMAGE_NAME}:${env.DOCKER_TAG} .
                """
            }
        }

        stage('Run Container') {
            steps {
                sh """
                docker run -d -p ${params.CONTAINER_PORT}:${params.CONTAINER_PORT} ${params.DOCKER_IMAGE_NAME}:${env.DOCKER_TAG}
                """
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh """
                echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                docker push ${params.DOCKER_IMAGE_NAME}:${env.DOCKER_TAG}
                """
            }
        }
    }

    post {
        failure {
            echo "❌ Build failed!"
        }
        success {
            echo "✅ Build succeeded!"
        }
    }
}



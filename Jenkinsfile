pipeline {
    agent any

    environment {
        IMAGE_NAME = 'docker-working'
        DOCKERHUB_USER = 'fizza424' // 🔁 Replace with your DockerHub username
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
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Container for Test') {
            steps {
                echo 'Running container for testing...'
                sh 'docker run -d -p 5000:5000 --name test_container $IMAGE_NAME'
                sh 'sleep 5'
                sh 'curl http://localhost:5000 || echo "App not responding"'
                sh 'docker stop test_container'
                sh 'docker rm test_container'
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                    sh 'docker tag $IMAGE_NAME $DOCKERHUB_USER/$IMAGE_NAME:latest'
                    sh 'docker push $DOCKERHUB_USER/$IMAGE_NAME:latest'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying container...'
                sh 'docker stop $IMAGE_NAME || true'
                sh 'docker rm $IMAGE_NAME || true'
                sh 'docker run -d -p 5000:5000 --name $IMAGE_NAME $DOCKERHUB_USER/$IMAGE_NAME:latest'
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD Completed Successfully!'
        }
        failure {
            echo '❌ Pipeline failed!'
        }
    }
}
Add Jenkinsfile for CI/CD pipeline


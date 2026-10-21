pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS = 'dockerhub-credentials'
        IMAGE_NAME = 'walid20066/my-app'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build("${IMAGE_NAME}:${BUILD_NUMBER}")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', "${DOCKER_CREDENTIALS}") {
                        app.push("${BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker stop my-app-running || true
                    docker rm my-app-running || true
                    docker run -d -p 3000:3000 --name my-app-running ${IMAGE_NAME}:latest
                '''
            }
        }
    }
}
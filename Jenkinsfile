pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') // configure DockerHub credentials in Jenkins
         // configure Slack webhook credentials
        IMAGE_NAME = "achrefs161/cv-onpage"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Achrefs16/cv-onpage.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-id') {
                        docker.image("${IMAGE_NAME}:latest").push()
                    }
                }
            }
        }
    }

}

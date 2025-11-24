pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials') // configure DockerHub credentials in Jenkins
        SLACK_WEBHOOK_URL = credentials('slack-webhook')    // configure Slack webhook credentials
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

    post {
        success {
            slackSend(channel: '#general', color: 'good', message: "✅ Pipeline succeeded: ${env.JOB_NAME} #${env.BUILD_NUMBER}", teamDomain: 'YOUR_TEAM', tokenCredentialId: 'slack-webhook')
        }
        failure {
            slackSend(channel: '#general', color: 'danger', message: "❌ Pipeline failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}", teamDomain: 'YOUR_TEAM', tokenCredentialId: 'slack-webhook')
        }
    }
}

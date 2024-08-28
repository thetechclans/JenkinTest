pipeline {
    agent any

    environment {
        // Define environment variables
        DOCKER_IMAGE = "laravel-app"
        DOCKER_REGISTRY_CREDENTIALS = 'Rilan_ksa' // ID for Docker Hub credentials in Jenkins
        // DOCKER_REPO = 'yourdockerhubusername/laravel-app'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                sh 'pwd'
                git branch: 'Jenkins', url: 'https://github.com/thetechclans/JenkinTest.git'
            }
        }
        stage('Build Docker Compose Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker-compose build'
                }
            }
        }
        stage('Docker Compose up') {
            steps {
                 script {
                    // Start the containers
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

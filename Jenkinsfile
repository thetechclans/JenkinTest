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
                // git branch: '11.x', url: 'https://github.com/thetechclans/JenkinTest.git'
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
                    // Remove existing conflicting container if it exists
                    sh 'docker ps -a --filter "name=my-laravel-mysql" --format "{{.ID}}" | xargs -r docker rm -f'
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

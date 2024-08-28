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
                // Clone the repository
                git branch: '11.x', url: 'https://github.com/thetechclans/JenkinTest.git'
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
                    sh "docker-compose up -d"
                }
            }
        }
        // stage('Push Docker Image') {
        //     steps {
        //         script {
        //             // Tag the Docker image
        //             sh "docker tag $DOCKER_IMAGE $DOCKER_REPO:latest"
        //             // Push the Docker image to Docker Hub
        //             sh "docker push $DOCKER_REPO:latest"
        //         }
        //     }
        // }
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

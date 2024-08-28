pipeline {
    agent any

    environment {
        // Define environment variables
        DOCKER_IMAGE = "laravel-app"
        DOCKER_REGISTRY_CREDENTIALS = 'dockerhub-credentials' // ID for Docker Hub credentials in Jenkins
        DOCKER_REPO = 'yourdockerhubusername/laravel-app'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                git branch: 'main', url: 'https://github.com/yourusername/your-laravel-repo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }
        stage('Docker Login') {
            steps {
                script {
                    // Log in to Docker Hub
                    sh "echo $DOCKER_REGISTRY_CREDENTIALS | docker login -u yourdockerhubusername --password-stdin"
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Tag the Docker image
                    sh "docker tag $DOCKER_IMAGE $DOCKER_REPO:latest"
                    // Push the Docker image to Docker Hub
                    sh "docker push $DOCKER_REPO:latest"
                }
            }
        }
        stage('Deploy to Server') {
            steps {
                // Deploy the image to the server
                sshagent(['your-server-ssh-credentials-id']) {
                    sh '''
                    ssh user@yourserver.com '
                        docker pull $DOCKER_REPO:latest &&
                        docker stop laravel-app || true &&
                        docker rm laravel-app || true &&
                        docker run -d --name laravel-app -p 9000:9000 $DOCKER_REPO:latest
                    '
                    '''
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images
            sh 'docker rmi $DOCKER_REPO:latest || true'
        }
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

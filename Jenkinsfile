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
                sh 'pwd'
                // git branch: 'Jenkins', url: 'https://github.com/thetechclans/JenkinTest.git',
                //     credentialsId: 'Rilan_ksa'
            }
        }

        // stage('Show Updated Files') {
        //     steps {
        //         script {
        //             def changes = sh(script: 'git rev-parse --is-inside-work-tree', returnStatus: true)
        //             if (changes == 0) {
        //                 sh 'echo "Updated files since the last build:"'
        //                 def previousCommit = sh(script: 'git rev-list --max-parents=0 HEAD', returnStdout: true).trim()
        //                 if (previousCommit) {
        //                     sh 'git diff --name-only ${previousCommit} HEAD'
        //                 } else {
        //                     echo 'No previous commits to compare with.'
        //                 }
        //             } else {
        //                 echo 'Not inside a Git repository.'
        //             }
        //         }
        //     }
        // }


        stage('Build Docker Compose Image') {
            steps {
                script {
                    // Check if the container already exists
                    def containerExists = sh(script: "docker ps -a --filter 'name=${DOCKER_IMAGE}' --format '{{.Names}}' | grep -w ${DOCKER_IMAGE}", returnStatus: true) == 0

                    // Skip build if container exists
                    if (containerExists) {
                        echo "Container '${DOCKER_IMAGE}' already exists. Skipping build."
                    } else {
                        echo "Container '${DOCKER_IMAGE}' does not exist. Building Docker image."
                        // Build the Docker image
                        sh 'docker-compose build'
                    }
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

        stage('Docker php Compose update') {
            steps {
                script {
                    sh "docker compose run php composer install"
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

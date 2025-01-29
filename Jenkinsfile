pipeline {
    agent { label 'docker' }
  }

    environment {
        // Use the credentials ID for Docker Hub
        DOCKER_HUB_CREDENTIALS = credentials('docker-creds')
    }

    stages {
        // Stage 1: Checkout code from Git
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-repo.git'
            }
        }

        // Stage 2: Build Docker image
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image and tag it with the build ID
                    dockerImage = docker.build("your-dockerhub-username/your-image-name:${env.BUILD_ID}")
                }
            }
        }

        // Stage 3: Push Docker image to Docker Hub
        stage('Push Docker Image') {
            steps {
                script {
                    // Authenticate with Docker Hub and push the image
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-creds') {
                        dockerImage.push()
                    }
                }
            }
        }

        // Stage 4: Push with 'latest' tag (optional)
        stage('Push Latest Tag') {
            steps {
                script {
                    // Tag the image as 'latest' and push it
                    dockerImage.push('latest')
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}

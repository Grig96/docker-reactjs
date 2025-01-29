pipeline {
    agent { label "docker-mac" }

    environment {
        DOCKER_IMAGE_NAME = "greeg/node-webapp"
        DOCKER_CLI_ACI = "0"
    }

    // ✅ Define dockerImage as a global variable
    def dockerImage

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // ✅ Assign the global variable instead of using "def"
                    dockerImage = docker.build("${DOCKER_IMAGE_NAME}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-creds') {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Push Latest Tag') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-creds') {
                        dockerImage.tag('latest')
                        dockerImage.push('latest')
                    }
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

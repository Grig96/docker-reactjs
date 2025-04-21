pipeline {
    agent { label "static-docker" }

    environment {
        DOCKER_IMAGE_NAME = "greeg/node-webapp"
        DOCKER_CLI_ACI = "0"
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
                    // ✅ Store dockerImage as an environment variable
                    env.DOCKER_IMAGE_TAG = "${DOCKER_IMAGE_NAME}:${env.BUILD_ID}"
                    docker.build(env.DOCKER_IMAGE_TAG)
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-creds') {
                        docker.image(env.DOCKER_IMAGE_TAG).push()
                    }
                }
            }
        }

        stage('Push Latest Tag') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-creds') {
                        docker.image(env.DOCKER_IMAGE_TAG).tag('latest')
                        docker.image(env.DOCKER_IMAGE_TAG).push('latest')
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

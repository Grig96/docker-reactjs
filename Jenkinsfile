pipeline {
    agent any

    environment {
        DOCKER_REGISTRY = 'docker.io'
        DOCKER_IMAGE_NAME = 'greeg/node-webapp'        
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
                tool name: 'docker', type: 'dockerTool'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ("${DOCKER_IMAGE_NAME}:latest") .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-creds', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USER')]) {
                    sh 'docker login  -u "${DOCKER_USER}" -p "${DOCKER_PASSWORD}"'
                        echo 'Logged in to Docker Hub'
                    }
                    }
                }
            }
        

        stage('Push Docker Image') {
            steps {
                script {
                    sh 'docker push ${DOCKER_IMAGE_NAME}'
                    }
                }
            }
        

        stage('Clean Up') {
            steps {
                script {
                    sh 'docker system prune -f'
                }
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

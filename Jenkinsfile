pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "ranjana061/devops-task"
        DOCKER_REGISTRY = "docker.io"
    }

    stages {
        stage('Clone Repository') {
            steps {
                // Clone your GitHub repository
                git 'https://github.com/pragatizone/DevOps-Task.git'
            }
        }

        stage('Login to Docker Registry') {
            steps {
                script {
                    // Log in to Docker Hub using stored credentials
                    withCredentials([usernamePassword(credentialsId: 'docker-credentials', 
                                                       usernameVariable: 'DOCKER_USERNAME', 
                                                       passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image with the name `ranjana061/devops-task`
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    docker.withRegistry("https://${DOCKER_REGISTRY}", 'docker-credentials') {
                        docker.image("${DOCKER_IMAGE}:latest").push()
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
    }
}

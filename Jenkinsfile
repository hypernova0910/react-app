pipeline {
    agent {
        // Run the pipeline stages inside a Docker container with Docker installed
        docker {
            image 'docker:latest'
            args '-v /var/run/docker.sock:/var/run/docker.sock'
        }
    }

    environment {
        BRANCH_NAME = 'main'
        DOCKER_IMAGE_NAME = 'react-app'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${BRANCH_NAME}", url: 'https://github.com/hypernova0910/react-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${DOCKER_IMAGE_NAME}")
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    // Stop any running containers with the same name
                    def containerId = sh(script: "docker ps -q --filter ancestor=${DOCKER_IMAGE_NAME}", returnStdout: true).trim()
                    if (containerId) {
                        sh "docker stop ${containerId}"
                    }

                    // Run the Docker container
                    sh "docker run -d ${DOCKER_IMAGE_NAME}"
                }
            }
        }
    }
}
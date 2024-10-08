pipeline {
    agent any

    environment {
        DOCKER_IMAGE_NAME = 'react-app'
        DOCKER_CONTAINER_NAME = 'react-app-container'
    }

    parameters {
        choice(name: 'BRANCH', choices: ['main', 'develop'], description: 'Branch to build')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${params.BRANCH}", url: 'https://github.com/hypernova0910/react-app.git'
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
                        sh "docker rm ${containerId}"
                    }

                    // Run the Docker container
                    sh "docker run -dp 4000:80 --name ${DOCKER_CONTAINER_NAME} ${DOCKER_IMAGE_NAME}"
                }
            }
        }
    }
}
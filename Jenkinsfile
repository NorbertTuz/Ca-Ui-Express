
pipeline {
    agent any
    tools {
        nodejs 'node-18.17.1'
    }
     environment {
        // Replace with your Docker image name
        DOCKER_IMAGE = 'your-docker-image-name:latest'
        
        // Replace with your Docker registry URL
        // Example for Docker Hub: 'https://index.docker.io/v1/'
        DOCKER_REGISTRY = 'HTTP://ec2-13-51-204-181.eu-north-1.compute.amazonaws.com/22'
        
        // Replace with your Docker registry credentials ID in Jenkins
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials-id'
    }
    stages {
        stage('Clone Repository') {
            steps {
                // Replace with your repository URL
                git 'https://github.com/NorbertTuz/Ca-Ui-Express.git'
            }
        }

        stage('Build') {
            steps {
                sh "npm install"
            }
        }

        stage('Unit Test') {
            steps {
                 sh "npm run test:unit"
            }
        }

        stage('Integration Test') {
            steps {
                sh "npm test:integration"
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry("${DOCKER_REGISTRY}", "${DOCKER_CREDENTIALS_ID}") {
                        docker.image("${DOCKER_IMAGE}").push()
                    }
                }
            }
        }

        stage('Deploy Docker Container') {
            steps {
                script {
                    docker.image("${DOCKER_IMAGE}").run("-d -p 80:80")
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}


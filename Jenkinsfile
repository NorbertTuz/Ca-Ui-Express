pipeline {
    agent any
    tools {
        nodejs 'node-18.17.1'
    }
    stages {

        stage('Build') {
            steps {
                sh "npm install"
            }
        }

        stage('Unit Test') {
            steps {
                sh "npm test:unit"
            }
        }

        stage('Integration Test') {
            steps {
                sh "npm test:integration"
            }
        }
    }
}




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
                 sh "npm run test:unit"
            }
        }
        stage('Integration Test') {
            steps {
                sh "npm run test:integration"
            }
        }
    }
}

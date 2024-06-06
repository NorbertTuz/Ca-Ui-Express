
pipeline {
    agent any
    tools {
        nodejs '18.17.0'
    }
    stages {
        stage('Build') {
            steps {
                sh "npm install"
            }
        }

        stage('Unit Test') {
            steps {
<<<<<<< HEAD
                sh "npm run test:unit"
=======
                 sh "npm run test:unit"
>>>>>>> 5dccfd524229c96d1ae2207413eb3ee52f79e49c
            }
        }
        stage('Integration Test') {
            steps {
                sh "npm run test:integration"
            }
        }
    }
}

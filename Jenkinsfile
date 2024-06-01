
pipeline {
    agent any

    tools {
        nodejs 'node-18.17.1'
    }

    environment {
        SCANNER_HOME = tool 'SonarQube Scanner' // Nazwa narzędzia SonarQube Scanner zdefiniowana w Jenkins
        NODE_ENV = 'production'
        SONARQUBE_URL = 'http://13.53.151.187:9000'
        SONARQUBE_TOKEN = 'sqa_ae190a086433bce768f00d34f826fbaed78b73b8'
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

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube Server') { // 'SonarQube Server' to nazwa serwera skonfigurowanego w Jenkins
                    sh """
                        ${SCANNER_HOME}/bin/sonar-scanner -X \
                        -Dsonar.projectKey=Ca-Ui-Express-app \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=${SONARQUBE_URL} \
                        -Dsonar.login=${SONARQUBE_TOKEN}
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 1, unit: 'MINUTES') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
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

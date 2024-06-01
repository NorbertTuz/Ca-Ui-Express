pipeline {
    agent any

    environment {
        SCANNER_HOME = tool 'SonarQube Scanner' // Nazwa narzędzia SonarQube Scanner zdefiniowana w Jenkins
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONARQUBE_URL = 'http://13.53.151.187:9000'
                SONARQUBE_TOKEN = 'sqa_ae190a086433bce768f00d34f826fbaed78b73b8'
            }
            steps {
                withSonarQubeEnv('SonarQube-Server') { 
                    sh "${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=sonar.projectKey=Ca-Ui-Express-app \
                        -Dsonar.sources=."
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

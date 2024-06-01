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
                SONARQUBE_TOKEN = 'sqa_0e7f16b83fdf26e775fbba609ce3705fdb374dbd'
            }
            steps {
                withSonarQubeEnv('SonarQube-Server') { 
                    sh "${SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.sonar.projectKey=Ca-Ui-Express-app \
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

pipeline {
    agent any
    tools {
        nodejs 'node-18.17.1'
    }
    environment {
        SCANNER_HOME = tool 'SonarQube Scanner' // Nazwa narzędzia SonarQube Scanner zdefiniowana w Jenkins
    }
    stages {
        stage('SCM') {
            steps {
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
                sh "npm run test:integration"
            }
        }

        stage('SonarQube Analysis') {
            environment {
                SONARQUBE_URL = 'http://13.53.151.187:9000'
                SONARQUBE_TOKEN = 'sqa_0e7f16b83fdf26e775fbba609ce3705fdb374dbd'
            }
            steps {
                withSonarQubeEnv('SonarQube-Server') {
                    sh """
                        ${SCANNER_HOME}/bin/sonar-scanner \
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
                    timeout(time: 5, unit: 'MINUTES') { // Zwiększony czas oczekiwania
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
            }
        }

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}


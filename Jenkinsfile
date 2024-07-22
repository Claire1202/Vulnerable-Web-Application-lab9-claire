pipeline {
    agent any
    environment {
        SONARQUBE_SCANNER_HOME = tool 'SonarQube' // Ensure this matches the configured scanner name
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/OWASP/Vulnerable-Web-Application.git'
            }
        }
        stage('Code Quality Check via SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') { // Ensure this matches the configured server name
                    sh """
                        ${env.SONARQUBE_SCANNER_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=OWASP \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://192.168.1.209:9000  \
                        -Dsonar.login=sqp_c4c6c5c000cfa6c71e3fa01042e12dbbe280cdc2
                    """
                }
            }
        }
    }
    post {
        always {
            recordIssues enabledForFailure: true, tool: sonarQube()
        }
    }
}

pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/OWASP/Vulnerable-Web-Application.git'
            }
        }
        stage('Install Node.js Modules') {
            steps {
                sh 'npm install'
            }
        }
        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = '/var/jenkins_home/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQube';
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=OWASP \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://192.168.1.209:9000 \
                            -Dsonar.token=sqp_3ffd9aaf57d34e57e08b7d4df6fbfe41540d785c \
                            -Dsonar.nodejs.executable=/usr/bin/node \
                        """
                    }
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

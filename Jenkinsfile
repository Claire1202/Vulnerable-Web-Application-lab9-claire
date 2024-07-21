pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/OWASP/Vulnerable-Web-Application.git'
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
                            -Dsonar.token=sqp_fd84b03267436623a75d380ce5b69eccfdd7d8c7 \
                            -Dsonar.ws.timeout=600 \
                            -Dsonar.ce.task.timeout=600
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

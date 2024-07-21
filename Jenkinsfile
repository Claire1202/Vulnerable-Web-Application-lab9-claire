pipeline {
    agent any
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
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
                            -Dsonar.token=sqp_f2fd6f36f0696b3fe3efc1e5aea40a63de0fefa0 \
                            -Dsonar.nodejs.executable=/usr/bin/node \
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

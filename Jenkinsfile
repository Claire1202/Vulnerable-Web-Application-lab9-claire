pipeline {
    agent any
    environment {
        NODE_HOME = '/usr/bin'
        PATH = "${NODE_HOME}:${env.PATH}"
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/OWASP/Vulnerable-Web-Application.git'
            }
        }
        // Skipping the 'Install Node.js Modules' stage since there's no package.json file
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
                            -Dsonar.nodejs.executable=/usr/bin/node
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

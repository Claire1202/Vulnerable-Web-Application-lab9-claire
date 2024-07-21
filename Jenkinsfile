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
                sh 'npm install is-number'
                sh 'npm install to-regex-range'
                sh 'npm install fill-range'
                sh 'npm install braces'
                sh 'npm install micromatch'
                sh 'npm install fast-glob'
                sh 'npm install globby'
                sh 'npm install @typescript-eslint/typescript-estree'
                sh 'npm install @typescript-eslint/type-utils'
                sh 'npm install @typescript-eslint/eslint-plugin'
            }
        }
        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube';
                    withSonarQubeEnv('SonarQube') {
                        sh """
                            ${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=OWASP \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://192.168.1.209:9000 \
                            -Dsonar.token=sqp_f2fd6f36f0696b3fe3efc1e5aea40a63de0fefa0 \
                            -Dsonar.ws.timeout=600
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

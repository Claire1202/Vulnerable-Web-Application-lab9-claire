pipeline {
    agent any
    stages {
        stage ('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/OWASP/Vulnerable-Web-Application.git'
            }
        }
        stage ('Install Node.js Packages') {
            steps {
                script {
                    // Install Node.js packages if a package.json file is present
                    if (fileExists('package.json')) {
                        sh 'npm install'
                    }
                }
            }
        }
        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube';
                    withSonarQubeEnv('SonarQube') {
                        // Use the correct path to your Node.js executable
                        def nodePath = 'C:/Program Files/nodejs/node.exe'
                        sh "${scannerHome}/bin/sonar-scanner \
                            -Dsonar.projectKey=OWASP \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://192.168.1.209:9000 \
                            -Dsonar.token=sqp_e026e2e73d6ab51b11cd03ee50947b3a26445fa5 \
                            -Dsonar.nodejs.executable=${nodePath}"
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

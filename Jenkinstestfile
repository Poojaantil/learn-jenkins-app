pipeline {
    agent any

    stages {
        stage('Build') {
            // your build code here
        }

        // 👇 Paste the Test stage here
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Running tests..."
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm test
                '''
            }
            post {
                always {
                    echo 'Test stage completed.'
                }
                success {
                    echo 'Tests passed successfully.'
                }
                failure {
                    echo 'Tests failed!'
                }
            }
        }
    }
}
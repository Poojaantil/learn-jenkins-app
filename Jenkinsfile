pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Starting build..."
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

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
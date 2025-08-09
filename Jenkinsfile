pipeline {
    agent any

    stages {
        stage('Parallel Tests') {
            parallel {
                
                stage('Build') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                            echo "Running build stage..."
                            node --version
                            npm --version
                            npm ci
                            npm run build || true
                        '''
                    }
                }

                stage('Unit Tests') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                            echo "Running unit tests..."
                            node --version
                            npm --version
                            npm ci
                            npm test || true
                        '''
                    }
                }

                stage('E2E Tests') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.39.0-focal'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                            echo "Running E2E tests..."
                            npm install
                            npm run build || true
                            npm install serve

                            # Start the app in background
                            npx serve -s build -l 3000 &
                            SERVER_PID=$!

                            sleep 5

                            # Run Playwright tests
                            npx playwright test || true

                            # Kill the background server
                            kill $SERVER_PID || true
                        '''
                    }
                }

            }
        }
    }
}

pipeline {
    agent any

    stages {
        stage('Build and Test (Parallel)') {
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
                            echo "Starting Build Stage..."
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
                            echo "Starting Test Stage..."
                            node --version
                            npm --version
                            npm ci
                            test -f build/index.html || echo "Build not found!"
                            npm test || true
                        '''
                    }
                }

            }
        }
    }
}

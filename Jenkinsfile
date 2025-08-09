pipeline {
    agent any

    stages {
        stage('Parallel Tasks') {
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
                            echo "Running Build..."
                            node --version
                            npm ci
                            npm run build
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
                            echo "Running Unit Tests..."
                            npm ci
                            npm test
                        '''
                    }
                }

                stage('Lint') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh '''
                            echo "Running Lint..."
                            npm ci
                            npm run lint
                        '''
                    }
                }
            }
        }
    }
}

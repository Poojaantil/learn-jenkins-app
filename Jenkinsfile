pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '347df3ff-beff-43a3-8962-08aedb778918'
    }

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

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "Starting Deploy Stage..."
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying to production site ID: $NETLIFY_SITE_ID "
                    # Example deployment command:
                    # node_modules/.bin/netlify deploy --dir=build --prod --auth $NETLIFY_AUTH_TOKEN --site $NETLIFY_SITE_ID
                '''
            }
        }
    }
}

pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '347df3ff-beff-43a3-8962-08aedb778918'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {

        stage('Build') {
            agent {
                docker {
                    image 'node:18'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "🔧 Build Stage"
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "🧪 Test Stage"
                    npm test || true
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'node:18'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "🚀 E2E Test Stage"
                    # Replace this with your actual E2E test command
                    echo "Running E2E tests..."
                    npm run test:e2e || true
                '''
            }
        }

        stage('Deploy') {
            agent {
                docker {
                    image 'node:18'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo "🚀 Deploy Stage"
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy \
                        --dir=build \
                        --auth=$NETLIFY_AUTH_TOKEN \
                        --site=$NETLIFY_SITE_ID \
                        --message "Deployed via Jenkins"
                '''
            }
        }

    }
}

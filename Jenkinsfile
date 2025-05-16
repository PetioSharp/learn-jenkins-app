pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'd2eebedd-1cd3-4383-91bd-3c8af39d764d'
    }

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
                    test -f build/index.html
                    npm test
                '''
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
                   npm install netlify-cli
                   node_modules/.bin/netlify --version
                   echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                '''
            }
        }

    }
    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'b3480744-329e-475a-a41d-d3c7afb5dbf9'
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
                sh 'ls -la'
                sh 'node --version'
                sh 'npm --version'
                sh 'npm ci'
                sh 'npm run build'
                sh 'ls -la'
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

            post {
                always {
                    junit 'test-results/**/junit.xml'
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
                    npm install netlify-cli
                    ./node_modules/.bin/netlify --version
                    echo "deploying to production, site id: $NETLIFY_SITE_ID"
                '''
            }
        }
    }
}
pipeline {
    environment {
        NETLIFT_SITE_ID = 'b3f377f8-434c-4e74-8b5b-314f60fe616f'
        NETLIFT_SITE_TOKEN = credentials('netlify-token')
    }
    
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
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la build/
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
            environment {
                NETLIFY_AUTH_TOKEN = credentials('netlify-token')
            }
            steps {
                sh '''
                    npm install -g netlify-cli
                    netlify --version
                    netlify deploy --dir=build --prod
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

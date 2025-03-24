pipeline {
    environment{
        NETLIFT_SITE_ID='b3f377f8-434c-4e74-8b5b-314f60fe616f' //LOL don't even try it , i'll be changing it once done 
    }
    
agent any
    stages {
    
        stage('build') {
            
            agent{
                docker{
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

           stage('Test'){
                   agent{
                docker{
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
                   stage('Depoly') {
            
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
              npm install netlify-cli 
              node_modules/.bin/netlify --version

                
                '''
            }
        }
        
    }

 
    post {
     always{
         junit 'test-results/junit.xml'
     }   
    }
}

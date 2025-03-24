pipeline {
    
agent any
    stages {
               steps {
                sh '''
                test -f build/index.html
                
                '''
            }
        }
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
        
    }
}

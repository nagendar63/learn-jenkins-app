pipeline {
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
                echo 'executing commands'
                sh '''
                    export NPM_CONFIG_CACHE=$(pwd)/.npm-cache
                    mkdir -p $NPM_CONFIG_CACHE
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
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
                echo 'executing netlify deploy'
                sh '''
                   npm install netlify-cli -g
                   netlify --version
                '''
            }
        }
        
    }

    

   
}

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
        stage('Test') {

            agent {
               
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

             steps {
                echo 'Test stage'
               sh '''
               test -f build/index.html
               npm test
               '''


             }

        }
        stage('E2E') {

            agent {
               
                docker {
                    image 'mcr.microsoft.com/playwright:v1.53.0-noble'
                    reuseNode true
                }
            }

             steps {
                echo 'Test stage'
               sh '''
                npm install -g serve
                serve -s build
                npx playwright test
               '''


             }

        }
        
    }

    post{
        always{
            junit 'tests-results/junit.xml'
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

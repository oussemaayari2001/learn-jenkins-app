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
                sh '''
                node --version
                npm --version
                ls -la
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

stage('Deployment') {
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

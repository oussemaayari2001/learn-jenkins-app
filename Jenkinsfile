pipeline {
    agent any
    environment {
        NETLIFY_SITE_ID = 'cb08456f-ff9a-4926-af5e-f5e6b9aa5834'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
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
              echo "deployment to $NETLIFY_SITE_ID succeded"
              node_modules/.bin/netlify status
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

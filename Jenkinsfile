pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '1af62d03-2eec-47ec-8deb-6d3a8a35b9c3'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage ('Run tests') {
            parallel {
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
                 post {
                    always {
                         junit 'test-results/junit.xml'
        }
    }
             }
        stage('E2E') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm --version
                    node --version
                    '''
                }
                 post {
                    always {
                        junit 'test-results/junit.xml'
        }
    }
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
                            node_modules/.bin/netlify --version
                            echo "Deploying to production. Site ID: $NETLIFY_SITE_ID"
                            node_modules/.bin/netlify status
                            node_modules/.bin/netlify deploy --dir=build --prod
                            '''
                }
             }
        
    }
   
}

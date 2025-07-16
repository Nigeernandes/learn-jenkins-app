pipeline {
    agent any

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
                            '''
                }
             }
        
    }
   
}

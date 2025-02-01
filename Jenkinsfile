pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
    }

    environment {
        NETLIFY_SITE_ID = '003307a6-3564-4385-b714-665c29c6fc7a'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }

        stage('Run Tests') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh '''
                        test -f build/index.html
                        npm test

                        '''
                    }
                }

                stage('Integration Tests') {
                    steps {
                        sh '''
                        echo "integration tests idk"
                        '''
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                npm install netlify-cli
                npx netlify --version
                echo "Site ID: $NETLIFY_SITE_ID"
                npx netlify status
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

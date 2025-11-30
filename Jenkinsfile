pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:20.11-bookworm-slim'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm cache clean --force
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'node:20.11-bookworm-slim'
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

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm i -g serve
                    serve -s build
                    npx playwright test
                '''
            }
        }
    }
    post {
        always {
            junit allowEmptyResults: true, testResults: 'test-results/**/*.xml'
        }
    }
}

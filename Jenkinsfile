pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm install --prefer-offline
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test') {
            steps {
                sh '''
                    [ -f /build/index.html ] && echo "Build successful" || echo "Build failed"
                    exit 1
                    npm run test
                '''
            }
        }
    }
}

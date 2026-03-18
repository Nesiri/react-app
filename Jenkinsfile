pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:20-alpine'  // Use LTS version instead of 25
                    reuseNode true
                }
            }
            steps {
                sh '''
                echo "Node version:"
                node --version
                echo "NPM version:"
                npm --version
                echo "Installing dependencies..."
                npm install
                echo "Building application..."
                npm run build
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
           steps {
                sh '''
                if test -f build/index.html; then
                    echo " running tests..."
                    npm test
                else
                    echo "❌ build/index.html not found! Build may have failed."
                    exit 1
                fi
                '''
           }
        }
    }
    
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
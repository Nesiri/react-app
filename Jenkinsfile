pipeline {
    agent any
    environment{
        NETLIFY_SITE_ID="3eb6739f-fa2f-4a22-bb67-3100d4a39ed6"
        NETLIFY_AUTH_TOKEN =credentials('tempo')
    }
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
                    echo " build/index.html not found! Build have failed."
                    exit 1
                fi
                '''
           }
        }

        stage("deploy"){
             agent {
                docker {
                    image 'node:20-alpine'
                    reuseNode true
                }
            }
           steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying to production. Site Id: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --prod --dir=build --no-build
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
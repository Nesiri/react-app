pipeline {
    agent {
        docker {
            image 'node:20-alpine'
            reuseNode true
        }
    }
    stages {
        stage('Build') {
            steps {
                sh '''
                node --version
                npm install
                npm run build
                '''
            }
        }
    }
}
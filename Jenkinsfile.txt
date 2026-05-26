pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t sample-nodejs-app .'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop sample-nodejs-app || true
                docker rm sample-nodejs-app || true

                docker run -d \
                --name sample-nodejs-app \
                -p 3000:3000 \
                sample-nodejs-app
                '''
            }
        }
    }
}
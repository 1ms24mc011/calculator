pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/1ms24mc011/calculator'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t calculator-app:latest .'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh '''
                docker stop calculator-container || true
                docker rm calculator-container || true
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8080:80 --name calculator-container calculator-app:latest'
            }
        }
    }

    post {
        success {
            echo "🚀 Deployment Successful! Visit: http://192.168.1.36:8080"
        }
    }
}

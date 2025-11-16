pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/1ms24mc011/calculator'
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

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar123', variable: 'sqa_9d8362f549ddbec70d4c3c58d1bee9d2fe240c5c')]) {
                    withSonarQubeEnv('MySonar') {
                        sh """
                            sonar-scanner \
                            -Dsonar.projectKey=calculator \
                            -Dsonar.sources=. \
                            -Dsonar.host.url=http://localhost:9000 \
                            -Dsonar.login=$sqa_9d8362f549ddbec70d4c3c58d1bee9d2fe240c5c
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo "🚀 Deployment Successful! Visit: http://192.168.1.36:8080"
        }
    }
}

pipeline {
  agent any

  stages {

    stage('Checkout Code') {
      steps {
        echo "Pulling latest code..."
        checkout scm
      }
    }

    stage('Install Dependencies') {
      steps {
        echo "Installing npm packages..."
        sh 'npm install'
      }
    }

    stage('Run Tests') {
      steps {
        echo "Running tests..."
        sh 'npm test -- --watchAll=false || true'
      }
    }

    stage('Build React App') {
      steps {
        echo "Building React app..."
        sh 'npm run build'
      }
    }

    stage('Archive Build Artifacts') {
      steps {
        echo "Archiving build folder..."
        archiveArtifacts artifacts: 'build/**', fingerprint: true
      }
    }
  }
}

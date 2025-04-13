pipeline {
  agent any
  environment {
    PATH = "/opt/homebrew/bin:/usr/local/bin:$PATH"
  }
  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build Frontend') {
      steps {
        dir('frontend') {
          sh 'npm install'
          sh 'ng build --configuration production'
        }
      }
    }
    stage('Test Backend') {
      steps {
        dir('backend') {
          sh 'npm install'
          sh 'npm run test'
        }
      }
    }
    stage('Build Docker Images') {
      steps {
        sh 'docker compose -f docker/docker-compose.yml build'
      }
    }
    stage('Run Containers') {
      steps {
        sh 'docker compose -f docker/docker-compose.yml up -d'
      }
    }
  }
  post {
    always {
      sh 'docker compose -f docker/docker-compose.yml down || true'
    }
  }
}
pipeline {
  agent any

  stages {
    stage('Checkout SCM') {
      steps {
        checkout scm
      }
    }
    
    stage('Build and package app') {
      environment {
        MAVEN_HOME = tool 'Maven'
        PATH = "$MAVEN_HOME/bin:${env.PATH}"
      }
      steps {
        sh 'mvn clean package'
      }
    }
    
    stage('Build Docker image') {
      steps {
        sh 'docker build -t myapp:latest .'
      }
    }
    
    stage('Run Docker container') {
      steps {
        sh 'docker run -d -p 8081:8081 myapp:latest'
      }
    }
  }

  post {
    always {
      cleanWs()
    }
  }
}

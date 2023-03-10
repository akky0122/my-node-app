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
    
    stage('Create Docker Image') {
      steps {
        script {
          def dockerfileContent = "FROM tomcat:9-jdk11-openjdk\nCOPY ./target/*.war /usr/local/tomcat/webapps/\nEXPOSE 8081\nCMD [\"catalina.sh\", \"run\"]"
          writeFile file: 'Dockerfile', text: dockerfileContent
        }
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

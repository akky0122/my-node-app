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
                // Write Dockerfile contents to a file
             sh 'echo "FROM tomcat:9-jdk11-openjdk\nCOPY /var/jenkins_home/workspace/build-myapp/target/myapp.war /usr/local/tomcat/webapps/\nEXPOSE 8081\nCMD [\"catalina.sh\", \"run\"]" > Dockerfile'

            }
    }
    
    stage('Build Docker image') {
      steps {
        sh 'docker build -t myapp:latest .'
      }
    }
    
    stage('Run Docker container') {
      steps {
        sh 'docker run -d -p 8080:8080 myapp:latest'
      }
    }
  }

  
}

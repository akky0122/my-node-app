pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Create Dockerfile') {
            steps {
                def dockerfileContent = "FROM tomcat:9-jdk11-openjdk\nCOPY /var/jenkins_home/workspace/build-myapp/target/myapp-1.0-SNAPSHOT.war /usr/local/tomcat/webapps/\nEXPOSE 8081\nCMD [\"catalina.sh\", \"run\"]"
                sh 'echo "$dockerfileContent" > Dockerfile'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }
        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 8080:8080 myapp:latest'
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

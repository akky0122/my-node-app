pipeline {
    agent {
        docker {
            image 'maven:3.8.3-jdk-11'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker run -d --name my-tomcat -p 8081:8080 tomcat:9.0'
                sh 'docker cp target/myapp.war my-tomcat:/usr/local/tomcat/webapps/'
            }
        }
    }
    post {
        always {
            sh 'docker stop my-tomcat'
            sh 'docker rm my-tomcat'
        }
    }
}

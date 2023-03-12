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
            environment {
                TOMCAT_CONTAINER = 'my-tomcat'
            }
            steps {
                sh 'docker run -d --name ${TOMCAT_CONTAINER} -p 8081:8081 tomcat:9.0'
                sh 'docker cp target/myapp.war ${TOMCAT_CONTAINER}:/usr/local/tomcat/webapps/'
            }
        }
    }
    post {
        always {
            sh 'docker stop ${TOMCAT_CONTAINER}'
            sh 'docker rm ${TOMCAT_CONTAINER}'
        }
    }
}

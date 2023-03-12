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
     /*   stage('Deploy') {
            steps {
                sh '''
                    # Copy WAR file to Tomcat webapps directory
                    # cp target/*.war /path/to/tomcat/webapps/
                   # curl --user <username>:<password> http://localhost:8080/manager/text/reload?path=/<app-name>
                    
                    # Copy WAR file to Linux EC2 instance
                    scp -i /path/to/ssh/key.pem target/*.war ec2-user@<ec2-instance-ip>:/home/ec2-user/
                '''
                */
                stage ('deploy to dev') {
             steps {
                  sshagent(['user-deployer']) {
                  sh "scp -o StrictHostKeyChecking=no */target/*.war ec2-user@3.111.40.171:/usr/local/tomcat/webapps/"
} } }
            
        
    }
}

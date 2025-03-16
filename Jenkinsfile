pipeline {
   agent any
   environment {
       TOMCAT_USER = 'admin'  
       TOMCAT_PASSWORD = 'admin123'  
       TOMCAT_URL = 'http://your-tomcat-server-ip:8080'  
       APP_NAME = 'NumberGuessGame'
   }
   stages {
       stage('Checkout Code') {
           steps {
               git branch: 'main', url: 'https://github.com/your-repo.git'
           }
       }
       stage('Build') {
           steps {
               sh 'mvn clean package'
           }
       }
       stage('Deploy to Tomcat') {
           steps {
               script {
                   def tomcatDeployUrl = "$TOMCAT_URL/manager/text/deploy?path=/$APP_NAME&update=true"
                   sh """
                   curl -u $TOMCAT_USER:$TOMCAT_PASSWORD -T target/$APP_NAME.war "$tomcatDeployUrl"
                   """
               }
           }
       }
   }
   post {
       success {
           echo 'Deployment successful!'
       }
       failure {
           echo 'Deployment failed! Check logs.'
       }
   }
}

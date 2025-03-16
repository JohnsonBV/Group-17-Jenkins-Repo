pipeline {

    agent any

    environment {        
        TOMCAT_USER = 'admin' 
        TOMCAT_PASSWORD = 'admin123' 
        TOMCAT_URL = 'http://3.87.36.102:8080' 
        APP_NAME = "NumberGuessGame"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    credentialsId: 'adf85567-a618-406c-b597-4add5379a357',
                    url: 'https://github.com/JohnsonBV/Group-17-Jenkins-Repo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def tomcatDeployUrl = "${TOMCAT_URL}/manager/text/deploy?path=/${APP_NAME}&update=true"
                    
                    sh """
                    curl -v -u ${TOMCAT_USER}:${TOMCAT_PASSWORD} -T target/${APP_NAME}.war "$tomcatDeployUrl"
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

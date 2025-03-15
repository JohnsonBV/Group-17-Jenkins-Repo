pipeline {

    agent any

    environment {

        APP_NAME = "NumberGuessGame"

    }

    stages {

        stage('Checkout Code') {

            steps {

                git branch: 'main',
                    
                    credentialsid: d84f6cf3-9967-40d4-83b6-282ed9538e1c
                
                    url: 'https://github.com/YOUR-USERNAME/NumberGuessGame.git'


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

        stage('Deploy') {

            steps {

                sh 'cp target/*.war /opt/tomcat/webapps/'

            }

        }

    }

}
 

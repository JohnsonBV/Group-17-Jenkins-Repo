pipeline {
    agent any

    environment {        
        TOMCAT_DIR = '/opt/tomcat/apache-tomcat-9.0.86/webapps' 
        APP_NAME = "NumberGuessGame"
        TOMCAT_URL = "http://3.87.36.102:8080"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    credentialsId: '76fdfa8c-b17a-457b-b004-00c2915fe4a5',
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

        stage('Undeploy from Tomcat') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: '3c6d307b-642f-4f13-98be-1526086453ed', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASSWORD')]) {
                        sh """
                        echo "Undeploying previous version..."
                        curl -v -u $TOMCAT_USER:$TOMCAT_PASSWORD "$TOMCAT_URL/manager/text/undeploy?path=/$APP_NAME" || true
                        """
                    }
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: '3c6d307b-642f-4f13-98be-1526086453ed', usernameVariable: 'TOMCAT_USER', passwordVariable: 'TOMCAT_PASSWORD')]) {
                        sh """
                        echo "Deploying to Tomcat..."

                        # Ensure Tomcat directory exists
                        sudo mkdir -p ${TOMCAT_DIR}

                        # Deploy using Tomcat Manager API
                        curl -v -u $TOMCAT_USER:$TOMCAT_PASSWORD \
                        --upload-file target/${APP_NAME}-1.0-SNAPSHOT.war \
                        "$TOMCAT_URL/manager/text/deploy?path=/$APP_NAME&update=true"

                        echo "Deployment Complete!"
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo '✅ Deployment successful!'
        }
        failure {
            echo '❌ Deployment failed! Check logs.'
        }
    }
}

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
                    withCredentials([usernamePassword(credentialsId: 'tomcat-deploy', usernameVariable: 'admin', passwordVariable: 'admin123')]) {
                        sh """
                        curl -v -u "$TOMCAT_USER:$TOMCAT_PASSWORD" "$TOMCAT_URL/manager/text/undeploy?path=/$APP_NAME" || true
                        """
                    }
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'tomcat-deploy', usernameVariable: 'admin', passwordVariable: 'admin123')]) {
                        sh """
                        echo "Deploying to Tomcat..."
                        cp target/${APP_NAME}-1.0-SNAPSHOT.war ${TOMCAT_DIR}/${APP_NAME}.war
                        systemctl restart tomcat || echo "Tomcat restart failed, check logs."
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

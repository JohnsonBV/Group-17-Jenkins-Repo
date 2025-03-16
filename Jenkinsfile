pipeline {
    agent any

    environment {        
        TOMCAT_DIR = '/opt/tomcat/apache-tomcat-9.0.86/webapps' 
        TOMCAT_URL = 'http://3.87.36.102:8080' 
        APP_NAME = "NumberGuessGame"
        TOMCAT_CREDENTIALS = credentials('3c6d307b-642f-4f13-98be-1526086453ed')  // Use stored credentials in Jenkins
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
                    sh """
                    echo "Undeploying existing app..."
                    curl -v -u ${TOMCAT_CREDENTIALS_USR}:${TOMCAT_CREDENTIALS_PSW} \
                    '${TOMCAT_URL}/manager/text/undeploy?path=/${APP_NAME}' || true
                    """
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    sh """
                    echo "Deploying to Tomcat..."

                    # Ensure Tomcat directory exists
                    sudo mkdir -p ${TOMCAT_DIR}

                    # Copy WAR file to Tomcat webapps directory
                    sudo cp target/${APP_NAME}-1.0-SNAPSHOT.war ${TOMCAT_DIR}/${APP_NAME}.war

                    # Restart Tomcat service
                    sudo systemctl restart tomcat || echo "Tomcat restart failed, check logs."

                    # Verify deployment
                    ls -lh ${TOMCAT_DIR}

                    echo "Deployment Complete!"
                    """
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

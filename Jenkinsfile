pipeline {

    agent any

    environment {        
        TOMCAT_DIR = '/opt/tomcat/apache-tomcat-9.0.86/webapps' 
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
                    def tomcatUndeployUrl = "${TOMCAT_URL}/manager/text/undeploy?path=/${APP_NAME}"

                    sh """
                    echo "Deploying to Tomcat..."

                    # Remove existing deployment
                    curl -v -u ${TOMCAT_USER}:${TOMCAT_PASSWORD} "$tomcatUndeployUrl" || true

                    # Copy WAR file to Tomcat webapps directory
                    sudo cp target/${APP_NAME}-1.0-SNAPSHOT.war ${TOMCAT_DIR}/${APP_NAME}.war

                    # Restart Tomcat service
                    sudo systemctl restart tomcat

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
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed! Check logs.'
        }
    }
}

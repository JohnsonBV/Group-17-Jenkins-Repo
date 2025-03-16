pipeline {
    agent any
 
    environment {
        TOMCAT_USER = 'admin'
        TOMCAT_PASSWORD = 'admin123'
        TOMCAT_URL = 'http://your-tomcat-server-ip:8080'
        APP_NAME = 'NumberGuessGame'
        WAR_FILE = "/var/lib/jenkins/workspace/NumberGuessGame/target/NumberGuessGame.war"
    }
 
    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    credentialsId: 'd84f6cf3-9967-40d4-83b6-282ed9538e1c',
                    url: 'https://github.com/JohnsonBV/Group-17-Jenkins-Repo.git'
            }
        }
 
        stage('Verify Workspace') {
            steps {
                sh 'ls -l'  // Ensure files exist after checkout
            }
        }
 
        stage('Clean Workspace') {
            steps {
                deleteDir()  // Remove old build files
            }
        }
 
        stage('Build') {
            steps {
                sh 'cd /var/lib/jenkins/workspace/NumberGuessGame && mvn clean package'
                sh 'ls -l target/'  // Confirm that the WAR file exists
                sh '[[ -f "$WAR_FILE" ]] || { echo "WAR file not found!"; exit 1; }'
            }
        }
 
        stage('Deploy to Tomcat') {
            steps {
                script {
                    def tomcatDeployUrl = "$TOMCAT_URL/manager/text/deploy?path=/$APP_NAME&update=true"
 
                    sh """
                    echo "Deploying to Tomcat..."
                    curl -v --fail -u $TOMCAT_USER:$TOMCAT_PASSWORD --upload-file $WAR_FILE "$tomcatDeployUrl" || { echo "Deployment failed!"; exit 1; }
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
 

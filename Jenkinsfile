pipeline {
    agent any

    tools {
        maven 'Maven'
        jdk 'jdk21'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
                echo 'Checking out the code from Git repository...'
                }
            }
        
        stage('Build') {
            steps {
                echo 'Building the project using Maven...'

                sh '''
                    cd /root/Jenkins-tomcat-sourcecode
                    mvn clean package
                '''
                }
            }

        stage('verify WAR file') {
            steps {
                echo 'Verifying the build war file...'
                sh '''
                    cd /root/Jenkins-tomcat-sourcecode
                    ls -l target/*.war
                '''
                }
            }

        stage('Deploy to Tomcat') {
            steps {

                sh '''
                    echo "Stopping Tomcat server..."

                    sudo -n /root/tomcat/bin/shutdown.sh || true

                    sleep 5

                    echo "Copying the WAR file to Tomcat webapps directory..."

                    sudo -n cp /root/Jenkins-tomcat-sourcecode/target/*.war /root/tomcat/webapps/

                    echo "Starting Tomcat server..."

                    sudo -n /root/tomcat/bin/startup.sh

                    sleep 10

                    echo "Deployment completed successfully."
                    
                    ls -l /root/tomcat/webapps/
                '''
                }
            }
        }
    post {
        success {
            echo 'Deployment completed.'
            }
        failure {
            echo 'Deployment failed.'
        }
    }
}       

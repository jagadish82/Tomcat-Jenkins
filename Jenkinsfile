pipeline {
    agent any

    tools {
        maven 'maven-3.6.3'
        jdk 'jdk21'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out the code from Git repository...'
                checkout scm
                }
            }
        
        stage('Build') {
            steps {
                echo 'Building the project using Maven...'
                sh 'mvn clean package'
                }
            }

        stage('verify WAR file') {
            steps {
                echo 'Verifying the build war file...'
                sh 'ls -l target/*.war'
                }
            }

        stage('Deploy to Tomcat') {
            steps {

                sh '''
                    echo "Stopping Tomcat server..."
                    sudo /root/tomcat/bin/shutdown.sh || true
                    sleep 5
                    echo "Copying the WAR file to Tomcat webapps directory..."
                    sudo cp /root/jenkins-tomcat-sourcecode/target/*.war /root/tomcat/webapps/
                    echo "Starting Tomcat server..."
                    sudo /root/tomcat/bin/startup.sh
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


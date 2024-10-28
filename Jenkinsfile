pipeline {
    agent any

    tools { 
        jdk 'JAVA_HOME'
        maven 'M2_HOME'
    }

    environment {
        NEXUS_URL = 'http://192.168.224.132:8081/repository/maven-releases/'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Anas-Hammou/activit-2.git'
            }
        }

        stage('Copy Managed File') {
            steps {
                script {
                    // Copy the managed settings.xml file to the workspace
                    sh 'cp $JENKINS_HOME/managed_files/package "$WORKSPACE/settings.xml"'
                }
            }
        }

        stage('Compile Project') {
            steps {
                // Skip all tests during the compile phase
                sh 'mvn clean compile -Dmaven.test.skip=true'
            }
        }

        stage('Package Application') {
            steps {
                // Use the copied settings file for packaging
                sh 'mvn -s "$WORKSPACE/settings.xml" package -Dmaven.test.skip=true'
            }
        }
    }
}

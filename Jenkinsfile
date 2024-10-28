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

        stage('Compile Project') {
            steps {
                // Skip all tests during the compile phase
                sh 'mvn clean compile -Dmaven.test.skip=true'
            }
        }

        stage('Package Application') {
            steps {
                // Skip all tests during the package phase
                sh 'mvn -s $MAVEN_SETTINGS package -Dmaven.test.skip=true'
            }
        }
    }
}

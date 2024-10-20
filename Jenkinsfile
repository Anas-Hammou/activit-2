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
                sh 'mvn package -Dmaven.test.skip=true'
            }
        }

        stage('Upload Artifact to Nexus') {
            steps {
                nexusArtifactUploader artifacts: [[
                    artifactId: 'timesheet-devops', 
                    classifier: '',
                    file: 'target/timesheet-devops-1.0.jar', 
                    type: 'jar'
                ]],
                credentialsId: '', // Set to empty string for anonymous access
                groupId: 'tn.esprit.spring.services', 
                nexusUrl: '192.168.224.132:8081',
                repository: 'maven-releases',
                version: '1.0',
                nexusVersion: 'nexus3',
                protocol: 'http'
            }
        }
    }

    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build or deployment failed!'
        }
    }
}

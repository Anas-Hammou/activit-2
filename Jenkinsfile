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
                // Updated Git repository URL
                git branch: 'master',
                url: 'https://github.com/Anas-Hammou/activit-2.git'
            }
        }

        stage('Compile Project') {
            steps {
                sh 'mvn clean compile -DskipTests'
            }
        }

        stage('Package Application') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Upload Artifact to Nexus') {
            steps {
                nexusArtifactUploader artifacts: [[
                    artifactId: 'timesheet-devops', // Updated artifactId based on the POM
                    classifier: '',
                    file: 'target/timesheet-devops-1.0.jar', // Ensure this matches your JAR file name
                    type: 'jar'
                ]],
                credentialsId: 'nexus-credentials',
                groupId: 'tn.esprit.spring.services', // Updated groupId based on the POM
                nexusUrl: 'http://192.168.224.132:8081',
                repository: 'maven-releases',
                version: '1.0',
                nexusVersion: 'nexus3', // Specify the Nexus version
                protocol: 'http' // Specify the protocol
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

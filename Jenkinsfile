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
                url: 'https://github.com/hwafa/timesheetproject.git'
            }
        }

        stage('Compile Project') {
            steps {
                sh 'mvn clean compile'
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
                    artifactId: 'timesheetproject',
                    classifier: '',
                    file: 'target/timesheetproject-1.0.0.jar',
                    type: 'jar'
                ]],
                credentialsId: 'nexus-credentials',
                groupId: 'com.example',
                nexusUrl: 'http://192.168.224.132:8081',
                repository: 'maven-releases',
                version: '1.0.0',
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

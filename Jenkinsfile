pipeline {
    agent any

    environment {
        IMAGE_NAME = "day5-maven-app"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Maven App') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh '''
                docker rm -f $IMAGE_NAME || true
                docker run -d --name $IMAGE_NAME $IMAGE_NAME
                '''
            }
        }
    }

    post {
        success {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}


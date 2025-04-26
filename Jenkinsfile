pipeline {
    agent any

    tools {
        maven 'Maven3'  // ✅ Must match exactly with Jenkins tool config
    }

    environment {
        IMAGE_NAME = 'backend-app'
        IMAGE_TAG = 'v1'
    }

    stages {
        stage('Clone Code') {
            steps {
                git url: 'https://github.com/farhanfist10/backend-maven.git', branch: 'main'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker rm -f backend-ci-container || true'  // Remove old container if exists
                sh 'docker run -d -p 8080:8080 --name backend-ci-container $IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }
}

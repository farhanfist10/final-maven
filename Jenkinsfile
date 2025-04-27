pipeline {
    agent any

    tools {
        maven 'Maven3'  // ✅ Should match Jenkins tool config
    }

    environment {
        IMAGE_NAME = 'backend-app'
        IMAGE_TAG = 'v1'
        NEXUS_IP = '54.87.222.116'  // ✨ Change this
        NEXUS_PORT = '8082'
    }

    stages {
        stage('Clone Code') {
            steps {
                git url: 'https://github.com/farhanfist10/final-maven.git', branch: 'main'
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

        stage('Tag Docker Image for Nexus') {
            steps {
                sh 'docker tag $IMAGE_NAME:$IMAGE_TAG $NEXUS_IP:$NEXUS_PORT/$IMAGE_NAME:$IMAGE_TAG'
            }
        }

        stage('Push Docker Image to Nexus') {
            steps {
                sh 'docker push $NEXUS_IP:$NEXUS_PORT/$IMAGE_NAME:$IMAGE_TAG'
            }
        }

        stage('Run Container (optional)') {
            steps {
                sh 'docker rm -f backend-ci-container || true'
                sh 'docker run -d -p 8080:8080 --name backend-ci-container $NEXUS_IP:$NEXUS_PORT/$IMAGE_NAME:$IMAGE_TAG'
            }
        }
    }
}
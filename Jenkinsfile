pipeline {
    agent any
    environment {
        IMAGE_NAME = 'simpleapp'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        CONTAINER_NAME = 'simpleapp-container'
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build Docker Image') {
            steps { sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .' }
        }
        stage('Stop Old Container') {
            steps { sh 'docker rm -f $CONTAINER_NAME || true' }
        }
        stage('Deploy') {
            steps { sh 'docker run -d --name $CONTAINER_NAME -p 8080:8080 $IMAGE_NAME:$IMAGE_TAG' }
        }
    }
}
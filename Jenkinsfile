pipeline {
    agent any
    environment {
        IMAGE_NAME = 'simpleapp'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        CONTAINER_NAME = 'simpleapp-container'
        DOCKERHUB_IMAGE = 'valli95/simpleapp'
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build Docker Image') {
            steps { sh 'docker build -t $IMAGE_NAME:$IMAGE_TAG .' }
        }
        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        docker tag $IMAGE_NAME:$IMAGE_TAG $DOCKERHUB_IMAGE:$IMAGE_TAG
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $DOCKERHUB_IMAGE:$IMAGE_TAG
                    '''
                }
            }
        }
        stage('Stop Old Container') {
            steps { sh 'docker rm -f $CONTAINER_NAME || true' }
        }
        stage('Deploy') {
            steps { sh 'docker run -d --name $CONTAINER_NAME -p 8080:8080 $IMAGE_NAME:$IMAGE_TAG' }
        }
    }
}
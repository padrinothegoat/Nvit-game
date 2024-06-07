pipeline {
    agent any
    environment {
        DOCKER_HUB_CREDENTIALS = 'dockerhub-repo'
        DOCKER_IMAGE_NAME = 'padrinothegoat/nvit-game-app'
        GITHUB_REPO_URL = 'https://github.com/padrinothegoat/Nvit-game.git'
        EC2_INSTANCE_IP = '52.39.238.216'
        EC2_PORT = '80'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git url: GITHUB_REPO_URL, branch: 'main'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE_NAME} .'
            }
        }
        stage('Push Docker Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-repo', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                    sh 'docker login -u $DOCKER_USERNAME -p $DOCKER_PASSWORD'
                    sh 'docker push ${DOCKER_IMAGE_NAME}'
                }
            }
        }
        stage('Deploy to EC2') {
            steps {
                sshagent(credentials: ['EC2-SSH-Key']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@${EC2_INSTANCE_IP} 'sudo docker run -d -p 80:80 ${DOCKER_IMAGE_NAME}'"
                }
            }
        }
    }
}

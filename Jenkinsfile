pipeline {
    agent any

    environment {
        // Define Docker Hub credentials
        DOCKER_HUB_CREDENTIALS = 'Docker/padrinothegoat'
        // Define Docker image name
        DOCKER_IMAGE_NAME = 'padrinothegoat/nvit-game-app' // Update 'your-image-name' with your desired image name
        // Define the github repository URL
        GITHUB_REPO_URL = 'https://github.com/padrinothegoat/Nvit-game.git'
    }

    stages {
        stage('Git Checkout') {
            steps {
                script {
                    echo 'Checking out code from github repository'
                }
                // Clone the Github repository and specify the develop branch
                git credentialsId: 'github-credentials', url: GITHUB_REPO_URL, branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image'
                    // List the contents of the workspace to debug and confirm the presence of Dockerfile
                    sh 'ls -la'
                    // Build Docker image using Dockerfile in the root directory
                    docker.build(DOCKER_IMAGE_NAME, '-f Dockerfile .')
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    echo 'Pushing Docker image to Docker Hub'
                    // Authenticate with Docker Hub
                    docker.withRegistry('https://registry.hub.docker.com', DOCKER_HUB_CREDENTIALS) {
                        // Push Docker image to Docker Hub
                        docker.image(DOCKER_IMAGE_NAME).push('latest')
                    }
                }
            }
        }
    }
}

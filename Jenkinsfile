pipeline {
    agent any

    environment {
        IMAGE_NAME = "myapp"
        DOCKER_HUB_USER = "your_dockerhub_username"
        DOCKER_HUB_PASS = credentials('dockerhub-credentials')  // Jenkins credential ID
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/yourusername/yourrepo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t $IMAGE_NAME:latest .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    sh 'echo $DOCKER_HUB_PASS | docker login -u $DOCKER_HUB_USER --password-stdin'
                }
            }
        }

        stage('Push Image to Docker Hub') {
            steps {
                script {
                    sh 'docker tag $IMAGE_NAME:latest $DOCKER_HUB_USER/$IMAGE_NAME:latest'
                    sh 'docker push $DOCKER_HUB_USER/$IMAGE_NAME:latest'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    // Stop old container if running
                    sh 'docker rm -f $IMAGE_NAME || true'
                    // Run new container
                    sh 'docker run -d -p 5000:5000 --name $IMAGE_NAME $DOCKER_HUB_USER/$IMAGE_NAME:latest'
                }
            }
        }
    }
}

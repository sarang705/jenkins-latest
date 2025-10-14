pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "myapp:latest"
        REGISTRY = "your-dockerhub-username/myapp"
    }

    stages {
        stage('Clone from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/yourusername/yourrepo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Run Tests (Optional)') {
            steps {
                sh 'echo "Running tests..."'
                // Add test scripts if any
            }
        }

        stage('Tag & Push to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker tag ${DOCKER_IMAGE} ${REGISTRY}:latest"
                        sh "docker push ${REGISTRY}:latest"
                    }
                }
            }
        }

        stage('Deploy (Optional)') {
            steps {
                echo "Deploying container..."
                // Example: sh "docker run -d -p 5000:5000 ${REGISTRY}:latest"
            }
        }
    }

    post {
        always {
            echo "Cleaning up..."
            sh "docker system prune -f"
        }
    }
}

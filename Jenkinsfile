pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'ashraf313/my-react-app'
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds'
    }

    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/azureuser180/my-react-app.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build Docker image
                    def image = docker.build("${DOCKER_IMAGE}:${IMAGE_TAG}")

                    // Push image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        image.push()              // Push tagged image
                        image.push('latest')      // Optionally push 'latest' tag
                    }
                }
            }
        }
    }

    post {
        success {
            echo "🎉 Docker image pushed: ${DOCKER_IMAGE}:${IMAGE_TAG}"
        }
        failure {
            echo "❌ Build or push failed. Check logs for details."
        }
    }
}


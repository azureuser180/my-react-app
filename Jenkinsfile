pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'ashraf313/my-react-app'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
        DOCKER_CREDENTIALS_ID = 'dockerhub-creds'
    }

    stages {
        stage('Clone') {
            steps {
                // ✅ Correct HTTPS URL format
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
                    docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
                        def image = docker.build("${DOCKER_IMAGE}:${IMAGE_TAG}")
                        image.push()
                        image.push('latest')
                    }
                }
            }
        }
    }

    post {
        success {
            echo "🎉 Image built and pushed to Docker Hub!"
        }
        failure {
            echo "❌ Build failed"
        }
    }
}

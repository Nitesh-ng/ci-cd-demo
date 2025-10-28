pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-login')
        IMAGE_NAME = 'nitesh181/ci-cd-demo'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Nitesh-ng/ci-cd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${IMAGE_NAME} .'
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh 'docker push ${IMAGE_NAME}'
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    // Stop and remove old container if running
                    sh 'docker rm -f ci-cd-demo || true'
                    // Run new container
                    sh 'docker run -d --name ci-cd-demo -p 3000:3000 ${IMAGE_NAME}'
                }
            }
        }
    }
}

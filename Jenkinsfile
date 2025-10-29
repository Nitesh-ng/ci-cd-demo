pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-login')
        DOCKER_IMAGE = "nitesh181/ci-cd-demo"
        CONTAINER_NAME = "ci-cd-app"
        APP_PORT = "8081"
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning repository from GitHub...'
                git branch: 'main', url: 'https://github.com/Nitesh-ng/ci-cd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    echo 'Pushing Docker image to Docker Hub...'
                    sh '''
                        echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                        docker push $DOCKER_IMAGE
                    '''
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    echo "Deploying container '$CONTAINER_NAME' on port $APP_PORT..."
                    sh '''
                        docker stop $CONTAINER_NAME || true
                        docker rm -f $CONTAINER_NAME || true
                        docker run -d -p ${APP_PORT}:80 --name $CONTAINER_NAME $DOCKER_IMAGE
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'CI/CD Pipeline executed successfully!'
            echo "Access your app at: http://localhost:${APP_PORT}"
        }
        failure {
            echo 'Pipeline failed! Please check the Jenkins console output for details.'
        }
        always {
            echo 'Cleaning up temporary Docker resources if any...'
        }
    }
}

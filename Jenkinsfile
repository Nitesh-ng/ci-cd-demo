pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-login')
        IMAGE_NAME = "niteshng/ci-cd-demo"  // change if your Docker Hub repo name differs
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/Nitesh-ng/ci-cd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh '''
                    echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                    docker push $IMAGE_NAME
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                    docker rm -f ci-cd-demo || true
                    docker run -d -p 5000:5000 --name ci-cd-demo $IMAGE_NAME
                '''
            }
        }
    }
}

pipeline {
    agent any

    environment {
        DOCKER_HOST = "unix:///home/user/.docker/desktop/docker.sock"
        IMAGE_NAME = "pranavi0906/pran"
    }

    stages {

        stage('Checkout from GitHub') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Bhuvaneshwari-bhu/k8n.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t k8n:${BUILD_NUMBER} .
                docker tag k8n:${BUILD_NUMBER} $IMAGE_NAME:${BUILD_NUMBER}
                '''
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                    echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                    docker push $IMAGE_NAME:${BUILD_NUMBER}
                    '''
                }
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop k8n-container || true
                docker rm k8n-container || true

                docker run -d -p 3000:8080 --name k8n-container $IMAGE_NAME:${BUILD_NUMBER}
                '''
            }
        }
    }
}

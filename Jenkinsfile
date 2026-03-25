pipeline {
    agent any

    stages {

        stage('Checkout from GitHub') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/pranavi0906/k8s.git'
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
                docker build -t k8s:${BUILD_NUMBER} .
                docker tag k8s:${BUILD_NUMBER} pranavi0906/k8s:${BUILD_NUMBER}
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push pranavi0906/k8s:${BUILD_NUMBER}'
            }
        }
}
}

pipeline {
    agent any

    environment {
        DOCKER_HOST = "unix:///home/user/.docker/desktop/docker.sock"
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
                docker tag k8n:${BUILD_NUMBER} pranavi0906/pran:${BUILD_NUMBER}
                '''
            }
        }

        stage('Docker Login & Push') {
            steps {
                sh '''
                export DOCKER_HOST=unix:///home/user/.docker/desktop/docker.sock
                export DOCKER_CONFIG=/tmp/docker-config

                rm -rf $DOCKER_CONFIG
                mkdir -p $DOCKER_CONFIG
                echo '{"auths":{}}' > $DOCKER_CONFIG/config.json

                # 🔥 Updated here
                echo "dckr_pat_Y6awuQ7qG-HTMEgH7YGJaGzh6_o" | docker login -u DOCKER_HUB_TOKEN --password-stdin

                docker tag k8n:${BUILD_NUMBER} pranavi0906/pran:${BUILD_NUMBER}
                docker push pranavi0906/pran:${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop k8n-container || true
                docker rm k8n-container || true

                docker run -d -p 3000:8080 --name k8n-container pranavi0906/pran:${BUILD_NUMBER}
                '''
            }
        }
    }
}

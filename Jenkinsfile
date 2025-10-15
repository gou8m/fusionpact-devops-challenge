pipeline {
    agent any

    environment {
        DOCKERHUB_REPO = "gou8m"
        DOCKERHUB_CREDENTIALS = 'dockerhub-id'   // The Jenkins credentials ID
    }

    stages {

        stage('Checkout Code') {
            steps {
                echo "Cloning source code from GitHub..."
                checkout scm
            }
        }

        stage('Build Backend Image') {
            steps {
                script {
                    echo "Building backend Docker image..."
                    sh 'docker build -t ${DOCKERHUB_REPO}/fusionpact-backend:latest ./backend'
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                script {
                    echo "Building frontend Docker image..."
                    sh 'docker build -t ${DOCKERHUB_REPO}/fusionpact-frontend:latest ./frontend'
                }
            }
        }

        stage('Push Images to Docker Hub') {
            steps {
                script {
                    echo "Logging into Docker Hub..."
                    withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh 'echo $PASSWORD | docker login -u $USERNAME --password-stdin'
                    }
                    echo "Pushing images..."
                    sh '''
                        docker push ${DOCKERHUB_REPO}/fusionpact-backend:latest
                        docker push ${DOCKERHUB_REPO}/fusionpact-frontend:latest
                    '''
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                script {
                    echo "Deploying application stack with docker-compose..."
                    sh '''
                        docker-compose down || true
                        docker-compose pull || true
                        docker-compose up -d --build
                    '''
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful."
        }
        failure {
            echo "Deployment failed. Check logs."
        }
    }
}

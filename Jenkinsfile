pipeline {
    agent any

    environment {
        DOCKER_USER = 'manjunath230'
        DOCKER_HUB_REPO = 'manjunath230'
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'develop', url: 'https://github.com/manju230/docker-jenkins-deployment.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh """
                    docker build -t ${DOCKER_USER}/${DOCKER_HUB_REPO}:${IMAGE_TAG} .
                """
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin

                        docker push ${DOCKER_USER}/${DOCKER_HUB_REPO}:${IMAGE_TAG}

                        docker tag ${DOCKER_USER}/${DOCKER_HUB_REPO}:${IMAGE_TAG} ${DOCKER_USER}/${DOCKER_HUB_REPO}:latest
                        docker push ${DOCKER_USER}/${DOCKER_HUB_REPO}:latest

                        docker logout
                    '''
                }
            }
        }

        stage('Cleanup') {
            steps {
                sh """
                    docker rmi ${DOCKER_USER}/${DOCKER_HUB_REPO}:${IMAGE_TAG} || true
                    docker rmi ${DOCKER_USER}/${DOCKER_HUB_REPO}:latest || true
                """
            }
        }
    }
}

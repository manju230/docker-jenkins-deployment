pipeline {
    agent any

    environment {
        DOCKER_HUB_USERNAME = 'manjunath230'
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
                    docker build -t ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPO}:${IMAGE_TAG} .
                """
            }
        }

        stage('Login & Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DH_USER',
                    passwordVariable: 'DH_PASS'
                )]) {
                    sh '''
                        echo "DEBUG USER = $DH_USER"   # TEMP debug

                        echo $DH_PASS | docker login -u $DH_USER --password-stdin

                        docker push ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPO}:${IMAGE_TAG}

                        docker tag ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPO}:${IMAGE_TAG} ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPO}:latest
                        docker push ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPO}:latest

                        docker logout
                    '''
                }
            }
        }

        stage('Cleanup') {
            steps {
                sh """
                    docker rmi ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPO}:${IMAGE_TAG} || true
                    docker rmi ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPO}:latest || true
                """
            }
        }
    }
}

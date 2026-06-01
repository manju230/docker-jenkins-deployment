pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'manju230'
        DOCKER_HUB_REPO = 'docker-jenkins-deployment'
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
                sh "docker build -t ${DOCKER_HUB_USER}/${DOCKER_HUB_REPO}:${IMAGE_TAG} ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push ${DOCKER_HUB_USER}/${DOCKER_HUB_REPO}:${IMAGE_TAG}
                    """
                }
            }
        }
    }
}

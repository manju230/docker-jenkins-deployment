pipeline {
    agent any

    environment {
        DOCKER_HUB_USER = 'manju230'   // your Docker Hub username
        DOCKER_HUB_REPO = 'docker-jenkins-deployment'
        IMAGE_TAG = "${env.BUILD_NUMBER}"  // auto-tag with build number
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'develop', url: 'https://github.com/manju230/docker-jenkins-deployment.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_HUB_USER}/${DOCKER_HUB_REPO}:${IMAGE_TAG} ."
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                        sh "echo $PASS | docker login -u $USER --password-stdin"
                        sh "docker push ${DOCKER_HUB_USER}/${DOCKER_HUB_REPO}:${IMAGE_TAG}"
                    }
                }
            }
        }
    }
}

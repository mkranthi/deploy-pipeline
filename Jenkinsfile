pipeline {
    agent any

    environment {
        DOCKER_CREDENTIALS_ID = 'docker-hub-cred' // ID of the Docker Hub credentials in Jenkins
    }

    stages {
        stage('Code Checkout') {
            steps {
                // Checkout your code from version control
                git branch: 'feature', url: 'https://github.com/mkranthi/deploy-pipeline.git'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                // Login to Docker Hub using credentials
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('pull Docker Image') {
            steps {
                // Tag and push the Docker image
                withCredentials([usernamePassword(credentialsId: "${DOCKER_CREDENTIALS_ID}", usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh 'docker pull $DOCKER_USER/itsoli-image:latest'
                }
            }
        }
        stage('Deploy manifestfile') {
            steps {
                
                    sh 'kubectl create -f itsoli-deployment.yaml'
                }
            }
        }
    }


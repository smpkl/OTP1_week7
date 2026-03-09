pipeline {
    agent any
    environment {
        PATH = "C:\\Program Files\\Docker\\Docker\\resources\\bin;${env.PATH}"
        DOCKERHUB_CREDENTIALS_ID = 'f41ec13c-4df1-43d3-b7ea-1ad77d8ff2e0'
        DOCKERHUB_REPO = 'gasdy/otp1_week7'
        DOCKER_IMAGE_TAG = 'latest'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/smpkl/OTP1_Week7.git'
            }
        }

        stage('Build') {
            steps {
                bat 'mvn clean install'
            }
        }

        stage('Build Docker Image') {
            steps {
              script {
                  docker.build("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}")
              }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
              script {
                  docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS_ID) {
                      docker.image("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                  }
              }
            }
        }

        stage('Deploy') {
            steps {
                script {
					bat 'docker compose down'
					bat 'docker compose up -d'
                }
            }
        }
    }
}
pipeline {

    agent any

    environment {
        IMAGE_NAME = "ramanuj/part-inventory-service"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git 'https://github.com/ramanujds/forvia-ci-repo'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME:$IMAGE_TAG ."
            }
        }

        stage('List Image') {
            steps {
                sh "docker images"
            }
        }
    }
}
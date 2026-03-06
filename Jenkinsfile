pipeline {

	agent any

	tools {
		dockerTool 'Docker'
	}

	environment {
		IMAGE_NAME = "ramanuj/part-inventory-service"
		IMAGE_TAG = "${BUILD_NUMBER}"
	}

	stages {

		stage('Source'){
			steps {
				echo 'Checking out source code from GitHub'
				git branch: 'main', url: 'https://github.com/ramanujds/forvia-ci-repo'
			}
		}

		stage('Build') {
			steps {
				echo 'Building docker image'
				sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
			}
		}

	}
}
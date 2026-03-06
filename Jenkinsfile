pipeline {

	agent any

	environment {
		IMAGE_NAME = "ram1uj/part-inventory-service"
		IMAGE_TAG = "${BUILD_NUMBER}"
	}

	stages {

		stage('Source') {
			steps {
				echo 'Checking out source code from GitHub'
				git branch: 'main', url: 'https://github.com/ramanujds/forvia-ci-repo'
			}
		}


		stage('Build Multi-Arch Image') {
			steps {
				echo 'Building multi-architecture Docker image'
				sh '''
                export PATH=$PATH:/usr/local/bin:/opt/homebrew/bin

                docker build \
                --platform linux/amd64 \
				-t ${IMAGE_NAME}:${IMAGE_TAG} \
				-t ${IMAGE_NAME}:latest \
				-f Dockerfile .
                '''
			}
		}

		stage('Verify Image') {
			steps {
				sh '''
                export PATH=$PATH:/usr/local/bin:/opt/homebrew/bin
                docker images | grep part-inventory-service
                '''
			}
		}

		stage('Push Image') {
			steps {
				withCredentials([usernamePassword(
					credentialsId: 'e4af9f44-e2b9-4253-b040-14b40090e1a6',
					passwordVariable: 'password',
					usernameVariable: 'username'
				)]) {
					sh '''
            export PATH=$PATH:/usr/local/bin:/opt/homebrew/bin

            echo "$password" | docker login -u "$username" --password-stdin

     	   	docker push ${IMAGE_NAME}:${IMAGE_TAG}
            docker push ${IMAGE_NAME}:latest

            docker logout
            '''
				}
			}
		}

		stage('Push to Kubernetes') {
			steps {
				withCredentials([file(credentialsId: 'aks-kubeconfig', variable: 'kubeconfig')]) {
					sh '''
					export PATH=$PATH:/usr/local/bin:/opt/homebrew/bin

					export KUBECONFIG=$kubeconfig

					kubectl set image deployment/part-inventory-service part-inventory-service=${IMAGE_NAME}:${IMAGE_TAG} --namespace=default

					kubectl rollout status deployment/part-inventory-service --namespace=default

					'''
				}
			}
		}

	}
}
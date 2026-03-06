pipeline {

	agent any

	environment {
		IMAGE_NAME = "ramanuj/part-inventory-service"
		IMAGE_TAG = "${BUILD_NUMBER}"
	}

	stages {

		stage('Source') {
			steps {
				echo 'Checking out source code from GitHub'
				git branch: 'main', url: 'https://github.com/ramanujds/forvia-ci-repo'
			}
		}

		//stage('Setup Buildx') {
		//	steps {
		//		sh '''
        //        export PATH=$PATH:/usr/local/bin:/opt/homebrew/bin
		//
        //        docker buildx create --name multiarch-builder --use || true
        //        docker buildx inspect --bootstrap
        //        '''
		//	}
		//}

		stage('Build Multi-Arch Image') {
			steps {
				echo 'Building multi-architecture Docker image'
				sh '''
                export PATH=$PATH:/usr/local/bin:/opt/homebrew/bin

                docker buildx build \
				--platform linux/amd64,linux/arm64 \
				-t ram1uj/part-inventory-service:${BUILD_NUMBER} \
				-t ram1uj/part-inventory-service:latest .
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

	}
}
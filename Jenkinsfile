pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'mohamed2252002/my-flask-app'
        DOCKER_CREDENTIALS_ID = 'docker-hub-credentials'
    }

    stages {
        
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
      
        stage('Test') {
            steps {
                echo 'Running Tests...'
                sh 'chmod +x hello.sh'
                sh './hello.sh'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                echo 'Building Image...'
                // Build the latest image
                sh "docker build -t ${DOCKER_IMAGE}:latest ."
            }
        }
        
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing Image...'
                withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    // 1. Login to Docker Hub
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    
                    // 2. Tag the image with the Build Number (e.g., v-1, v-2)
                    sh "docker tag ${DOCKER_IMAGE}:latest ${DOCKER_IMAGE}:v-${BUILD_NUMBER}"

                    // 3. Push both tags (latest & versioned)
                    sh "docker push ${DOCKER_IMAGE}:latest"
                    sh "docker push ${DOCKER_IMAGE}:v-${BUILD_NUMBER}"
                }
            }
        }
    }
}

pipeline {
    agent any
    
    environment {
       
        DOCKER_IMAGE = 'mohamed2252002/my-flask-app'
        // تأكد ان الاسم ده هو نفس الـ ID اللي عملناه في Jenkins Credentials
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
                sh "docker build -t ${DOCKER_IMAGE}:latest ."
            }
        }

       
        stage('Push to Docker Hub') {
            steps {
                echo 'Pushing Image...'
                withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS_ID, passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                    sh "docker push ${DOCKER_IMAGE}:latest"
                }
            }
        }
    }
}


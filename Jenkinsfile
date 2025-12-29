pipeline {
    agent any

    environment {
        // 1. Set your Docker Hub username and image name here
        DOCKER_IMAGE = "mohamed2252002/my-flask-app"
        DOCKER_TAG = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    // 2. Your repository URL
                    url: 'https://github.com/mohamed2252002/jenkins-private-task.git',
                    // Ensure this ID matches your GitHub Credentials ID in Jenkins
                    credentialsId: 'github' 
            }
        }

        stage('Run Hello World') {
            steps {
                sh 'ls -l'
                // Ensure the hello.sh file exists in your repository
                sh 'chmod +x hello.sh'
                sh './hello.sh'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    // 3. The Docker Hub credentials ID we configured (with the Token)
                    credentialsId: 'docker-hub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                        docker logout
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully & Docker image pushed 🚀'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
    }
}

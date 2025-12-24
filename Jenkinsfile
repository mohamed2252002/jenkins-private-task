pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh './myscript.sh'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing phase...'
            }
        }
    }
}

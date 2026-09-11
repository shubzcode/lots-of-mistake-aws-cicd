pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub...'
            }
        }

        stage('Verify Docker') {
            steps {
                bat 'docker --version'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t azure-docker-cicd:%BUILD_NUMBER% .'
            }
        }

        stage('Verify Image') {
            steps {
                bat 'docker images azure-docker-cicd'
            }
        }
    }

    post {
        success {
            echo '✅ CI pipeline completed successfully!'
        }

        failure {
            echo '❌ CI pipeline failed. Check the console output.'
        }
    }
}
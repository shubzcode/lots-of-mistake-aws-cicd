pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'shubzcode/azure-docker-cicd'
    }

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
                bat 'docker build -t %DOCKER_IMAGE%:%BUILD_NUMBER% .'
            }
        }

stage('Docker Login') {
    steps {
        withCredentials([
            usernamePassword(
                credentialsId: 'dockerhub-credentials',
                usernameVariable: 'DOCKER_USER',
                passwordVariable: 'DOCKER_PASSWORD'
            )
        ]) {
            bat '''
                echo %DOCKER_PASSWORD% | docker login -u "%DOCKER_USER%" --password-stdin
            '''
        }
    }
}

        stage('Push Docker Image') {
            steps {
                bat 'docker push %DOCKER_IMAGE%:%BUILD_NUMBER%'
            }
        }

        stage('Verify Image') {
            steps {
                bat 'docker images %DOCKER_IMAGE%'
            }
        }
    }

    post {
        success {
            echo '✅ CI/CD image build and push completed successfully!'
        }

        failure {
            echo '❌ Pipeline failed. Check the console output.'
        }
    }
}
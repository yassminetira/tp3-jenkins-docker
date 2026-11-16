
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker') {
            steps {
                bat 'docker build -t tp3-site:jenkins .'
            }
        }

        stage('Test Docker') {
            steps {
                bat 'docker image inspect tp3-site:jenkins'
                bat 'docker run --rm tp3-site:jenkins nginx -t'
            }
        }

        stage('Push Docker Hub') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_TOKEN'
                    )
                ]) {
                    bat '''
                    echo %DOCKER_TOKEN% | docker login -u %DOCKER_USER% --password-stdin
                    docker tag tp3-site:jenkins %DOCKER_USER%/tp3-site:latest
                    docker push %DOCKER_USER%/tp3-site:latest
                    docker logout
                    '''
                }
            }
        }
    }
}


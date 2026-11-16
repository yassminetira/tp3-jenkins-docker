
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
    }
}

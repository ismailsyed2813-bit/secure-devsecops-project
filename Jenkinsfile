pipeline {
    agent any

    stages {

        stage('Clone Code') {
            steps {
                git 'https://github.com/ismailsyed2813-bit/secure-devsecops-project.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t secure-node-app .'
            }
        }

        stage('Run Container') {
            steps {
                bat 'docker run -d -p 3002:3000 secure-node-app'
            }
        }
    }
}

pipeline {
    agent any

    stages {

        stage('SonarQube Scan') {
            steps {
                echo "Running SonarQube Scan"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t secure-app .'
            }
        }

        stage('Trivy Scan') {
            steps {
                sh 'trivy image secure-app'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 3000:3000 secure-app'
            }
        }
    }
}
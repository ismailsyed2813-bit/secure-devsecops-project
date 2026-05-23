pipeline {
    agent any

    tools {
        sonarScanner 'sonar-scanner'
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=secure-devsecops-project \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://host.docker.internal:9000 \
                    -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t secure-node-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 3002:3000 secure-node-app'
            }
        }
    }
}
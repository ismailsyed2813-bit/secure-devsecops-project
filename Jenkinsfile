pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token')
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                    ${SCANNER_HOME}/bin/sonar-scanner \
                    -Dsonar.projectKey=secure-devsecops-project \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://host.docker.internal:9000 \
                    -Dsonar.login=$SONAR_TOKEN
                    '''
                }
            }
        }
    stage('Trivy Scan') {
       steps {
          sh 'trivy image secure-node-app || true'
       }
    }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t secure-node-app .'
            }
        }

        stage('Run Container') {
            steps {
                sh 'docker run -d -p 3003:3000 secure-node-app'
            }
        }
    }
}
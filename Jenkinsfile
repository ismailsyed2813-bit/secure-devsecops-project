pipeline {
    agent any

    environment {
        SONAR_HOME = tool 'sonar-scanner'
    }

    stages {

        stage('Checkout Code') {
    steps {
        git 'https://github.com/ismailsyed2813-bit/secure-devsecops-project.git'
    }
}

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                    $SONAR_HOME/bin/sonar-scanner \
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

        stage('Trivy Scan') {
            steps {
                sh 'trivy image secure-node-app || true'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f secure-node-container || true
                docker run -d -p 3000:3000 --name secure-node-container secure-node-app
                '''
            }
        }
    }
}
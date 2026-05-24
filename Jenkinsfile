pipeline {
    agent any

    stages {

        stage('SonarQube Scan') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                    /opt/sonar-scanner/bin/sonar-scanner \
                    -Dsonar.projectKey=secure-devsecops-project \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://host.docker.internal:9000 \
                    -Dsonar.login=squ_2b64efbca4929b4645f034780dbcbf1a41626f1c
                    '''
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ismail2813/secure-devsecops-app:latest .'
            }
        }

        stage('Trivy Scan') {
            steps {
                sh 'trivy image ismail2813/secure-devsecops-app:latest'
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {

                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push ismail2813/secure-devsecops-app:latest
                    '''
                }
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f secure-container || true
                docker run -d --name secure-container -p 3001:3000 ismail2813/secure-devsecops-app:latest
                '''
            }
        }
    }
}
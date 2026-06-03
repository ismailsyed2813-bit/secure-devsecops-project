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


        stage('OWASP Dependency Check') {
            steps {
                dependencyCheck(
                    additionalArguments: '--scan .',
                    odcInstallation: 'OWASP-DC'
                )

                dependencyCheckPublisher(
                    pattern: '**/dependency-check-report.xml'
                )
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
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {

                    sh '''
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker push ismail2813/secure-devsecops-app:latest
                    '''
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml
                kubectl apply -f service.yaml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                kubectl rollout status deployment/secure-node-app
                kubectl get pods
                kubectl get svc
                '''
            }
        }
    }

    post {

        success {
            echo '✅ DevSecOps Pipeline Executed Successfully!'
        }

        failure {
            echo '❌ DevSecOps Pipeline Failed!'
        }

        always {
            cleanWs()
        }
    }
}
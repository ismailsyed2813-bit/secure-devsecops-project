pipeline {
    agent any

    stages {

        stage('SonarQube Scan') {
    steps {
        withSonarQubeEnv('sonar-server') {
            sh '''
            sonar-scanner \
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
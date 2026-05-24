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
        sh 'docker run -d -p 3000:3000 --name secure-node-container secure-node-app'
    }
}
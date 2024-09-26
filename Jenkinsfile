pipeline {
    agent {
        docker {
            image 'node:16-alpine' // or any image that has Docker
            args '-v /var/run/docker.sock:/var/run/docker.sock' // to access the host's Docker
        }
    }
    stages {
        stage('Test Docker') {
            steps {
                sh '/usr/bin/docker --version' // Test if Docker is available
            }
        }
    }
}

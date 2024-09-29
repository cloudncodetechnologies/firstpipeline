pipeline {
    agent {
        // Use a Docker Maven agent for the build
        docker {
            image 'maven:3.8.4-openjdk-17'
            args '-v /root/.m2:/root/.m2'  // Bind the local Maven repository to speed up builds
        }
    }

    environment {
        // Replace 'sonar' with your SonarQube server name in Jenkins
        SONARQUBE_SERVER = 'Sonarqube-test'
        // Optionally specify additional SonarQube parameters (like project key, token, etc.)
        SONAR_LOGIN = credentials('Secret text')  // Use credentials from Jenkins
    }

    stages {
        stage('Checkout Github') {
            steps {
                git branch: 'main', credentialsId: '1b256f95-2b17-4dfa-b6a6-9be1b3d86afd', url: 'https://github.com/cloudncodetechnologies/firstpipeline.git'
                sh 'cat /var/lib/jenkins/workspace/First\\ pipeline/Jenkinsfile'
            }
        }


        stage('Build with Maven') {
            steps {
                script {
                    // Run Maven to build the project
                    sh 'mvn clean install'
                }
            }
        }

        stage('SonarQube Scan') {
            steps {
                script {
                    // Perform the SonarQube analysis
                    withSonarQubeEnv(SONARQUBE_SERVER) {
                        sh 'mvn sonar:sonar -Dsonar.login=$SONAR_LOGIN'
                    }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    // Wait for SonarQube quality gate result
                    timeout(time: 5, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build and SonarQube quality check passed.'
        }
        failure {
            echo 'Build or SonarQube quality check failed.'
        }
    }
}

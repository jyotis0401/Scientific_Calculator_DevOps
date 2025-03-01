pipeline {
    agent any
    environment {

            GITHUB_REPO_URL = 'https://github.com/jyotis0401/Scientific_Calculator_DevOps.git'
     }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${GITHUB_REPO_URL}"
            }
            
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }
}
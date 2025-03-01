pipeline {
    agent any
    environment {
            MAVEN_HOME = '/opt/homebrew/Cellar/maven/3.9.9/libexec'
            GITHUB_REPO_URL = 'https://github.com/jyotis0401/Scientific_Calculator_DevOps.git'
            PATH = '${MAVEN_HOME}/bin:${env.PATH}'
     }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${GITHUB_REPO_URL}"
            }

        }
        stage('Build') {
            steps {
                sh 'mvn -version'
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
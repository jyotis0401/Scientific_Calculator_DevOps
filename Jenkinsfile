pipeline {
    agent any
    environment {
            GITHUB_REPO_URL = 'https://github.com/jyotis0401/scientific_calculator_devops.git'
            DOCKER_IMAGE_NAME = 'jyotis0401/scientific_calculator_devops'
            DOCKER_CREDENTIALS = 'Docker_Credential'
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
        stage('Build Docker Image') {
                    steps {
                        script {
                            sh 'docker --version'
        //                     sh "docker build -t ${DOCKER_IMAGE_NAME} ."
                            sh "docker build -t ${DOCKER_IMAGE_NAME} ."

                        }
                    }
                }
                stage('Docker Hub Login') {
                    steps {
                        withCredentials([usernamePassword(credentialsId: env.DOCKER_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                            sh '''
                                echo "Logging into Docker Hub..."
                                echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin || echo "Docker login failed!"
                            '''
                        }
                    }
                }
                stage('Push Docker Images') {
                    steps {
                        script {
                            sh "docker push ${DOCKER_IMAGE_NAME}"
                        }
                    }
                }
                stage('Deploy using Ansible') {
                    steps {
                        withCredentials([usernamePassword(credentialsId: 'local-cred', usernameVariable: 'ANSIBLE_USER', passwordVariable: 'ANSIBLE_PASS')]) {
                              sh '''
                                    ansible-playbook -i inventory.ini deploy.yml --extra-vars "ansible_user=$ANSIBLE_USER ansible_ssh_pass=$ANSIBLE_PASS"
                                 '''
                        }
        //                    sh 'ansible-playbook -i inventory.ini deploy.yml'
                    }
                }
    }
}
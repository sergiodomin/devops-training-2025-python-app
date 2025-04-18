pipeline {
    agent none

    stages {
        stage('Checkout') {
            agent { label 'docker' }
            steps {
                git branch: 'feat/base', url: 'https://github.com/sergiodomin/devops-training-2025-python-app.git'
            }
        }

        stage('Install Dependencies') {
            agent {
                docker {
                    image 'python:3.10'
                    args '-u root:root'
                }
            }
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            agent {
                docker {
                    image 'python:3.10'
                    args '-u root:root'
                }
            }
            steps {
                sh 'pytest'
            }
        }

        stage('Build Docker Image') {
            agent any // Usa el host de Jenkins
            steps {
                sh 'docker build -t my-python-app .'
            }
        }

        stage('Push to DockerHub') {
            agent any
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push my-python-app
                    '''
                }
            }
        }
    }
}

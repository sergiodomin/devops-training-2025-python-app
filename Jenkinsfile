pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'feat/base', url: 'https://github.com/sergiodomin/devops-training-2025-python-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    docker.image('python:3.10').inside('-u root:root') {
                        sh 'pip install -r requirements.txt'
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    docker.image('python:3.10').inside('-u root:root') {
                        sh 'pytest'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t my-python-app .'
            }
        }

        stage('Push to DockerHub') {
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

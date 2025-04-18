pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'my-flask-app-dc'
        DOCKER_TAG = 'latest'
        SONARQUBE_URL = 'http://sonarqube:9000'
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/sergiodomin/devops-training-2025-python-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:${DOCKER_TAG}")
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Aquí puedes agregar comandos para ejecutar tus pruebas
                    // Por ejemplo, usando pytest:
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").inside {
                        sh 'pytest tests'
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    // Ejecutar análisis con SonarQube
                    docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").inside {
                        sh 'mvn sonar:sonar -Dsonar.host.url=${SONARQUBE_URL}'
                    }
                }
            }
        }

        stage('Push Docker Image to DockerHub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        docker.image("${DOCKER_IMAGE}:${DOCKER_TAG}").push()
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Aquí puedes agregar el paso para el despliegue, por ejemplo:
                    // sh './deploy.sh'
                }
            }
        }
    }
    
    post {
        success {
            echo "Pipeline successfully completed!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}

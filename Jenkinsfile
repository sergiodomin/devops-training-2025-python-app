pipeline {
    agent {
        docker {
            image 'python:3.10'
            args '-u root:root'  // Esto da permisos si se requiere instalar algo
        }
    }
    stages {
        stage('Checkout') {
            steps {
                // Descargar el código del repositorio
                git branch: 'feat/base', url: 'https://github.com/sergiodomin/devops-training-2025-python-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Instalar las dependencias necesarias
                script {
                    sh 'pip install -r requirements.txt'
                }
            }
        }

        stage('Run Tests') {
            steps {
                // Ejecutar las pruebas
                script {
                    sh 'pytest'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                // Construir la imagen Docker
                script {
                    sh 'docker build -t my-python-app .'
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                // Subir la imagen Docker a DockerHub
                script {
                    sh 'docker push my-python-app'
                }
            }
        }
    }
}

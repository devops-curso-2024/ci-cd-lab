pipeline {
    agent any
    environment {
        REPO_URL = 'https://github.com/devops-curso-2024/ci-cd-lab.git'  // URL del repositorio de GitHub
        IMAGE_NAME = 'my-flask-app'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: "${env.REPO_URL}", branch: 'main'  // Clona el repositorio desde GitHub
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} .'  // Construye la imagen Docker
            }
        }
        stage('Run') {
            steps {
                script {
                    // Verifica si el contenedor existe
                    def containerExists = sh(script: "docker ps -a --filter name=${IMAGE_NAME} --format '{{.Names}}'", returnStdout: true).trim()
                    if (containerExists) {
                        // Detiene y elimina el contenedor si existe
                        sh "docker stop ${IMAGE_NAME}"
                        sh "docker rm ${IMAGE_NAME}"
                    }
                    // Ejecuta un nuevo contenedor
                    sh "docker run -d --name ${IMAGE_NAME} -p 5000:5000 ${IMAGE_NAME}:${BUILD_NUMBER}"
                }
            }
        }
    }
}


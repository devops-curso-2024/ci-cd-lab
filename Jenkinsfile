pipeline {
    agent any
    environment {
        REPO_URL = 'https://github.com/devops-curso-2024/ci-cd-lab.git'  # URL del repositorio de GitHub.
        IMAGE_NAME = 'my-flask-app'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: "${env.REPO_URL}", branch: 'main'  # Clona el repositorio desde GitHub.
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${env.BUILD_NUMBER} .'  # Construye la imagen Docker.
            }
        }
        stage('Run') {
            steps {
                sh '''
                if docker ps -a | grep ${IMAGE_NAME}; then
                  docker stop ${IMAGE_NAME}
                  docker rm ${IMAGE_NAME}
                fi
                docker run -d --name ${IMAGE_NAME} -p 5000:5000 ${IMAGE_NAME}:${env.BUILD_NUMBER}
                '''  # Despliega el contenedor.
            }
        }
    }
}


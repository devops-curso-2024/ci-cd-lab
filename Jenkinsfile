pipeline {
    agent any
    environment {
        REPO_URL = 'https://github.com/devops-curso-2024/ci-cd-lab.git'  
        IMAGE_NAME = 'my-flask-app'
    }
    stages {
        stage('Checkout') {
            steps {
                git url: "${env.REPO_URL}", branch: 'main'  
            }
        }
        stage('Build') {
            steps {
                sh 'docker build -t ${IMAGE_NAME}:${env.BUILD_NUMBER} .'  
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
                '''  
            }
        }
    }
}


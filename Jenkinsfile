pipeline {
    agent any
    environment {
        REPO_URL = 'https://github.com/devops-curso-2024/ci-cd-lab.git'
        IMAGE_NAME = 'my-flask-app'
    }
    stages {
        stage('Checkout') {
            steps {
                script {
                    echo "Clonando el repositorio"
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: "${env.REPO_URL}"]]])
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    def imageTag = "${env.IMAGE_NAME}:${env.BUILD_NUMBER}"
                    echo "Construyendo la imagen Docker ${imageTag}"
                    sh "docker build -t ${imageTag} ."
                }
            }
        }
        stage('Run') {
            steps {
                script {
                    def containerId = sh(script: "docker ps -aqf 'name=${env.IMAGE_NAME}'", returnStdout: true).trim()
                    if (containerId) {
                        echo "Deteniendo y eliminando el contenedor existente ${containerId}"
                        sh "docker stop ${containerId}"
                        sh "docker rm ${containerId}"
                    }
                    def imageTag = "${env.IMAGE_NAME}:${env.BUILD_NUMBER}"
                    echo "Ejecutando un nuevo contenedor ${imageTag}"
                    sh "docker run -d --name ${env.IMAGE_NAME} -p 5000:5000 ${imageTag}"
                }
            }
        }
    }
    post {
        always {
            script {
                def runningContainer = sh(script: "docker ps -qf 'name=${env.IMAGE_NAME}'", returnStdout: true).trim()
                if (runningContainer) {
                    echo "El contenedor ${env.IMAGE_NAME} está en ejecución."
                } else {
                    echo "El contenedor ${env.IMAGE_NAME} no está en ejecución."
                }
            }
        }
    }
}


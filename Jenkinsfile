pipeline {
    agent any
    environment {
        REPO_URL = 'https://github.com/devops-curso-2024/ci-cd-lab.git'
        IMAGE_NAME = 'my-flask-app'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: "${env.REPO_URL}"]]])
            }
        }
        stage('Build') {
            steps {
                script {
                    def imageTag = "${env.IMAGE_NAME}:${env.BUILD_NUMBER}"
                    sh "docker build -t ${imageTag} ."
                }
            }
        }
        stage('Run') {
            steps {
                script {
                    def containerId = sh(script: "docker ps -aqf 'name=${env.IMAGE_NAME}'", returnStdout: true).trim()
                    if (containerId) {
                        sh "docker stop ${containerId}"
                        sh "docker rm ${containerId}"
                    }
                    def imageTag = "${env.IMAGE_NAME}:${env.BUILD_NUMBER}"
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
                    echo "Container ${env.IMAGE_NAME} is running."
                } else {
                    echo "Container ${env.IMAGE_NAME} is not running."
                }
            }
        }
    }
}


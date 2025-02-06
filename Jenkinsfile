pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "gopi4678/dockertask2"
        DOCKER_CREDENTIALS = "dockerhub-credential"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/gopi4678/pro1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $DOCKER_IMAGE:latest ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: dockerhub-credential, usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    }
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    sh "docker push $DOCKER_IMAGE:latest"
                }
            }
        }

        stage('Cleanup') {
            steps {
                sh "docker rmi $DOCKER_IMAGE:latest"
            }
        }
    }

    post {
        success {
            echo "Docker image successfully built and pushed to Docker Hub!"
        }
        failure {
            echo "Build failed!"
        }
    }
}

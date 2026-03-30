pipeline {
    agent any

    environment {
        DOCKER_USER = "fathimadiya111"
    }

    stages {

        stage('Clone') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    tag = "v1.${BUILD_NUMBER}"
                    sh "docker build -t my-app:${tag} ."
                    sh "docker tag my-app:${tag} ${DOCKER_USER}/my-app:${tag}"
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh "docker push ${DOCKER_USER}/my-app:${tag}"
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker stop my-container || true'
                sh 'docker rm my-container || true'
                sh "docker run -d -p 80:80 --name my-container ${DOCKER_USER}/my-app:${tag}"
            }
        }
    }
}

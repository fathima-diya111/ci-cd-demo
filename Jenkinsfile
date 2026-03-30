pipeline {
    agent any

    environment {
        DOCKER_USER = "fathimadiya111"
        IMAGE_NAME = "my-app"
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/fathima-diya111/ci-cd-demo.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    def tag = "v1.${BUILD_NUMBER}"
                    env.TAG = tag
                    sh "docker build -t $DOCKER_USER/$IMAGE_NAME:$TAG ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push') {
            steps {
                sh "docker push $DOCKER_USER/$IMAGE_NAME:$TAG"
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker stop my-container || true'
                sh 'docker rm my-container || true'
                sh "docker run -d -p 80:80 --name my-container $DOCKER_USER/$IMAGE_NAME:$TAG"
            }
        }
    }
}

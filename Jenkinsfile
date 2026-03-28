pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                git 'https://github.com/fathima-diya111/ci-cd-demo.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("my-app")
                }
            }
        }

        stage('Deploy Container') {
            steps {
                script {
                    sh 'docker stop my-container || true'
                    sh 'docker rm my-container || true'
                    sh 'docker run -d -p 80:80 --name my-container my-app'
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "aswathks/flask-ci-cd"
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:latest .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_IMAGE:latest'
            }
        }

        stage('Deploy Container') {
            steps {
                sh 'docker rm -f flaskcontainer || true'
                sh 'docker run -d -p 5000:5000 --name flaskcontainer $DOCKER_IMAGE:latest'
            }
        }
    }
}
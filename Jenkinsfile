pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flaskapp:latest .'
            }
        }

        stage('Stop Old Container') {
            steps {
                sh 'docker rm -f flaskcontainer || true'
            }
        }

        stage('Run Docker Container') {
            steps {
                sh 'docker run -d -p 5000:5000 --name flaskcontainer flaskapp:latest'
            }
        }
    }
}

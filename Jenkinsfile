pipeline {
    agent any

    environment {
        IMAGE_NAME = 'celia2000/tp-jenkins-python'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Tests Python') {
            steps {
                script {
                    docker.image('python:3.10').inside('-u 0:0') {
                        sh 'pip install -r requirements.txt'
                        sh 'python app.py'
                        sh 'pytest'
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:latest .'
            }
        }

        stage('Login DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $IMAGE_NAME:latest'
            }
        }

        stage('Deploy Test') {
            steps {
                sh 'docker rm -f test-python || true'
                sh 'docker run -d --name test-python $IMAGE_NAME:latest'
            }
        }
    }
}
pipeline {
    agent any

    environment {
        
        IMAGE_NAME = "michell0313/my_docker:latest"
        CONTAINER_NAME = "ai-model-api"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Docker Build') {
            steps {
                
                sh 'docker build -t ${IMAGE_NAME} .'
            }
        }

        stage('Debug Path') {
            steps {
                sh 'pwd'
                sh 'ls -F'
            }
        }


        stage('Deploy') {
            steps {
                
                sh '''
                docker rm -f ${CONTAINER_NAME} || true
                docker compose down || true
                docker compose up -d
                '''
            }
        }

        stage('Test API') {
            steps {
                
                sh '''
                sleep 5
                curl http://localhost:8000 || true
                curl "http://localhost:8000/predict?value=1.5" || true
                '''
            }
        }
    }
}
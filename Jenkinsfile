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

        stage('Docker Build & Push') {
            steps {
               
                sh '''
                docker build -t ${IMAGE_NAME} .
                '''
                
               
                withCredentials([usernamePassword(credentialsId: 'dockerhub-token', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh '''
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push ${IMAGE_NAME}
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
               
                sh '''
                docker rm -f ${CONTAINER_NAME} || true
                docker pull ${IMAGE_NAME}
                docker compose down || true
                docker compose pull
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
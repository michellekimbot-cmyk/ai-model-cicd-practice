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
                script {
                    def myImage = docker.build("${IMAGE_NAME}")

                    docker.withRegistry("https://index.docker.io/v1/",'dockerhub-token')
                        myImage.push()


                }
            }
        }
    

        stage('Deploy') {
            steps {
                sh '''
                docker rm -f $CONTAINER_NAME || true
                docker pull $IMAGE_NAME
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
                curl http://localhost:8000
                curl "http://localhost:8000/predict?value=1.5"
                '''
            }
        }
    }
}
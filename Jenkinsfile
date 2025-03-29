pipeline {
    agent any
    environment {
        IMAGE_NAME = 'myproject:latest'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }
        stage('Run Migrations') {
            steps {
                sh 'docker run --rm $IMAGE_NAME python manage.py migrate'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'docker run --rm $IMAGE_NAME python manage.py test'
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker stop myproject || true'
                sh 'docker rm myproject || true'
                sh 'docker run -d -p 8001:8001 --restart unless-stopped --name myproject $IMAGE_NAME'
            }
        }
        stage('Cleanup') {
            steps {
                sh 'docker system prune -f'
            }
        }
    }
}

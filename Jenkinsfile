pipeline {
    agent any

    environment {
        IMAGE_NAME = "adarshroyals/hellodocker:latest"
    }

    stages {

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/AdarshRoyals/helloDocker.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deploymentsvc.yaml'
                sh 'kubectl rollout restart deployment/hellodocker-deployment'
            }
        }

        stage('Verify Deployment') {
            steps {
                sh 'kubectl get pods'
                sh 'kubectl get svc'
            }
        }
    }

    post {
        success {
            echo 'Deployment completed successfully!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}

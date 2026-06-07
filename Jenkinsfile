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
                sh '''
                export KUBECONFIG=/home/jenkins/.kube/config 
                cd /var/jenkins_home/workspace/Automated
                kubectl apply -f deploymentsvc.yaml --validate=false
                kubectl rollout restart deployment/hellodocker-deployment
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                export KUBECONFIG=/home/jenkins/.kube/config 
                kubectl get pods
                kubectl get svc
                '''
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

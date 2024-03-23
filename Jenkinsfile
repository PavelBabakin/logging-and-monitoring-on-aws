pipeline {
    agent any
    environment {
        // Replace 'username/yourapp' with your actual Docker Hub repo name
        DOCKER_IMAGE = 'backup0/simple-php-app'
        // Use the Docker Hub credentials ID configured in Jenkins
        REGISTRY_CREDENTIALS_ID = 'docker-hub-credentials'
        // Use the SSH key credentials ID for your EC2 instance configured in Jenkins
        DEPLOY_SERVER_CREDENTIALS_ID = 'simple-php-app-deploy-key'
        // EC2 Instance user and IP. Adjust as necessary
        DEPLOY_SERVER_USER = 'ubuntu'
        DEPLOY_SERVER_IP = '54.93.188.169'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/PavelBabakin/logging-and-monitoring-on-aws'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(env.DOCKER_IMAGE + ':latest')
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', env.REGISTRY_CREDENTIALS_ID) {
                        docker.image(env.DOCKER_IMAGE + ':latest').push('latest')
                    }
                }
            }
        }
        stage('Deploy on EC2') {
            steps {
                sshagent(credentials: [env.DEPLOY_SERVER_CREDENTIALS_ID]) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${env.DEPLOY_SERVER_USER}@${env.DEPLOY_SERVER_IP} '
                        docker pull ${env.DOCKER_IMAGE}:latest && 
                        docker stop simple-php-app || true && 
                        docker run --rm -d --name simple-php-app -p 80:80 ${env.DOCKER_IMAGE}:latest'
                    """
                }
            }
        }
    }
}
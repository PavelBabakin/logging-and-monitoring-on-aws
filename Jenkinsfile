pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/PavelBabakin/logging-and-monitoring-on-aws'
            }
        }
        stage('Deploy') {
            steps {
                sshagent(credentials: ['deploy-ssh']) {
                    sh "mkdir -p ~/.ssh"
                    sh "ssh-keyscan 54.93.188.169 >> ~/.ssh/known_hosts"
                    sh "scp -o StrictHostKeyChecking=no -r * ubuntu@54.93.188.169:/var/www/html"
                    sh "scp -r * ubuntu@54.93.188.169:/var/www/html"
                }
            }
        }
    }
}

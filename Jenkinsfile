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
                    sh "scp -r * ec2-user@54.93.188.169:/var/www/html"
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        DEPLOY_SERVER = 'ubuntu@15.206.151.227' // Replace with your EC2 instance IP or DNS
        SSH_KEY = credentials('ssh') // Jenkins credentials ID for your SSH key
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/Tirthu/simple-web-app.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Build step - can include compilation if needed'
                // Example: Build your application if required
                // sh 'make build'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Add the host to known_hosts to avoid host key verification issues
                    sh """
                    ssh-keyscan -H 15.206.151.227 >> ~/.ssh/known_hosts
                    scp -i ${SSH_KEY} -r * ${DEPLOY_SERVER}:/var/www/html/
                    ssh -i ${SSH_KEY} ${DEPLOY_SERVER} 'sudo systemctl restart httpd'
                    """
                }
            }
        }
    }
}

pipeline {
    agent any

    environment {
        SSH_KEY = credentials('ssh') // Replace with your actual SSH key ID from Jenkins credentials
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Clone Repository') {
            steps {
                script {
                    // Ensure the .ssh directory exists and add the host to known hosts
                    sh '''
                    mkdir -p ~/.ssh
                    ssh-keyscan -H 15.206.151.227 >> ~/.ssh/known_hosts
                    '''
                }
                git url: 'https://github.com/Tirthu/simple-web-app.git', branch: 'master', credentialsId: 'your-git-credentials-id' // Replace with your actual Git credentials ID
            }
        }

        stage('Build') {
            steps {
                echo 'Build step - can include compilation if needed'
                // Add build steps here if needed
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Deploy to the server
                    sh '''
                    scp -i ${SSH_KEY} -r Jenkinsfile index.html ubuntu@15.206.151.227:/var/www/html/
                    ssh -i ${SSH_KEY} ubuntu@15.206.151.227 sudo systemctl restart apache2
                    '''
                }
            }
        }
    }

    post {
        failure {
            echo 'The build failed.'
        }
    }
}

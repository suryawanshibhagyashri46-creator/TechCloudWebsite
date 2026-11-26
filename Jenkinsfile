pipeline {
 
    agent any
 
    stages {
 
        stage('Checkout Code') {
            steps {
                echo 'Downloading source code from GitHub'
                checkout scm
            }
        }
 
        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image'
 
                sh 'docker build -t techcloud-website:${BUILD_NUMBER} .'
            }
        }
 
        stage('Deploy Application') {
            steps {
                echo 'Deploying application on EC2'
 
                sh '''
                    docker rm -f techcloud-app || true
 
                    docker run -d \
                    --restart unless-stopped \
                    -p 80:80 \
                    --name techcloud-app \
                    techcloud-website:${BUILD_NUMBER}
                '''
            }
        }
 
        stage('Verify Deployment') {
            steps {
                echo 'Checking Docker container'
 
                sh 'docker ps'
 
                sh 'curl -f http://localhost'
            }
        }
    }
 
    post {
        success {
            echo 'Website deployed successfully!'
        }
 
        failure {
            echo 'Deployment failed. Check Jenkins console output.'
        }
    }
}
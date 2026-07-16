pipeline {
    agent any
 
    environment{
        IMAGE_NAME = "task-traker"
        IMAGE_TAG = "${BUILD_NUMBER}"
    }
 
    stages{
 
        stage('checkout'){
            steps{
                git branch: 'main', credentialsId: 'git-hub', url: 'https://github.com/Daya9096/traker-1.git'
                    
            }
        }
 
        stage('Build docker image'){
            steps{
                sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
            }
        }
 
        stage('Deploy using docker compose'){
            steps{
                sh """
                docker-compose down --remove-orphans || true
                docker-compose up -d
                """
 
            }
        }
    }
}

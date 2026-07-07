pipeline {
    agent any
 
    environment {
        IMAGE_NAME = "task-tracker"
        CONTAINER_NAME = "task-tracker"
    }
 
    stages {
        stage('SCM Pull') {
            steps {
                checkout scm
            }
        }
 
        stage('Install Dependencies and Run Tests') {
            steps {
                sh 'npm install'
                sh 'npm test'
            }
        }
 
        stage('Build') {
            steps {
                // Using double quotes so Jenkins environment variables interpolate correctly
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }
 stage('Deploy') {
    steps {
        // Point down explicitly to the compose file so it knows exactly what to destroy
        sh 'docker compose -f docker-compose.yml -p task-tracker down --remove-orphans || true'
        
        // Force recreation of the container to prevent any naming collision bugs
        sh 'docker compose -f docker-compose.yml -p task-tracker up -d --force-recreate'
    }
}
 
        stage('Curl') {
            steps {
                sh 'sleep 10'
                sh 'echo "===== HOME ====="'
                sh 'curl -f http://localhost:3000/'
                sh 'echo ""'
 
                sh 'echo "===== HEALTH ====="'
                sh 'curl -f http://localhost:3000/health'
                sh 'echo ""'
 
                sh 'echo "===== TASKS ====="'
                sh 'curl -f http://localhost:3000/api/tasks'
                sh 'echo ""'
            }
        }
 
        stage('Cleanup') {
            steps {
                // REMOVED "docker compose down" so the app stays running!
                sh 'docker image prune -f'
                cleanWs()
            }
        }
    }
 
    post {
        always {
            echo 'Pipeline Finished'
        }
        success {
            echo 'Application deployed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

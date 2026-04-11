pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/hiramustafabaig/Disaster-Management-App'
            }
        }

        stage('Stop & Clean Old Containers') {
            steps {
                sh '''
                docker rm -f ci_web_app ci_mongo_db || true
                docker compose -f docker-compose.jenkins.yml down --remove-orphans || true
                '''
            }
        }

        stage('Build & Run CI Containers') {
            steps {
                sh '''
                docker compose -f docker-compose.jenkins.yml up -d --build
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                echo "Containers running:"
                docker ps
                echo "Checking port mappings:"
                docker ps --format "table {{.Names}}\t{{.Ports}}"
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished (success or failure)'
        }

        success {
            echo 'CI/CD pipeline executed successfully'
        }

        failure {
            echo 'Pipeline failed — check logs'
        }
    }
}

pipeline {
    agent any

    environment {
        COMPOSE_PROJECT_NAME = "monitoring"
    }

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Images') {
            steps {
                echo '🔨 Building Docker images...'
                sh 'docker-compose build'
            }
        }

        stage('Start Services') {
            steps {
                echo '🚀 Starting Prometheus, Grafana, and App...'
                sh 'docker-compose up -d'
            }
        }

        stage('Wait for Services to Start') {
            steps {
                echo '⏳ Giving services time to initialize...'
                sh 'sleep 15'
            }
        }

        stage('Health Checks') {
            steps {
                echo '✅ Checking service health...'
                sh '''
                    curl

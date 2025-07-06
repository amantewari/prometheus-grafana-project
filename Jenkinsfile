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
                   docker-compose exec -T app curl -f http://localhost:5000 || (echo "❌ Flask app failed" && exit 1)
                   docker-compose exec -T grafana curl -f http://localhost:3000 || (echo "❌ Grafana failed" && exit 1)
                   docker-compose exec -T prometheus curl -f http://localhost:9090 || (echo "❌ Prometheus failed" && exit 1)
                 '''
     }
  }

        stage('Run Tests') {
            when {
                expression { fileExists('test.sh') }
            }
            steps {
                echo '🧪 Running app tests...'
                sh './test.sh'
            }
        }

        stage('Teardown') {
            steps {
                echo '🧹 Stopping and removing containers...'
                sh 'docker-compose down'
            }
        }
    }

    post {
        always {
            echo '🧽 Clean up any remaining containers or volumes...'
            sh 'docker-compose down -v'
        }
    }
}

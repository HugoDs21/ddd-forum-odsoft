pipeline {
    agent any
    stages {
        stage('Verify installations') {
            steps {
                sh '''
                    docker version
                    docker compose version
                    node -v
                '''
            }
        }
        stage('Start containers') {
            steps {
                sh 'docker-compose up -d --wait'
                sh 'docker compose ps'
            }
        }
    }
    post {
        always {
            sh 'docker-compose down --remove-orphans -v'
            sh 'docker compose ps'
        }
    }
}
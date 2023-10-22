#!/usr/bin/env groovy

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
                sh 'docker compose up -d --wait'
                sh 'docker compose ps'
            }
        }

        stage('Create database') {
            steps {
                sh 'npm run db:create:dev'
                sh 'npm run migrate:dev'
            }
        }

        stage('Start backend and run tests') {
            parallel {
                stage('Start backend') {
                    steps {
                        timeout(time: 5, unit: 'MINUTES') {
                            sh 'npm run start:dev'
                        }
                    }
                }
                stage('Run tests') {
                    steps {
                        retry(5) {
                            sh 'npm run testUnit'
                        }
                    }
                }
            }
        }
    }
    post {
        always {
            sh 'docker compose down --remove-orphans -v'
            sh 'docker compose ps'
        }
    }
}
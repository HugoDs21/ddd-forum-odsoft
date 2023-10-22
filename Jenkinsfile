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

        // stage('Prepare environment') {
        //     steps {
        //         sh 'cp .env.template .env'
        //     }
        // }

        stage('Start containers') {
            steps {
                sh 'docker compose -f docker-composeRunTest.yml up -d --wait'
                sh 'docker compose ps'
            }
        }

        // stage('Create database') {
        //     steps {
        //         sh 'npm run setup:dev'
        //     }
        // }

        // stage('Start backend and run tests') {
        //     parallel {
        //         stage('Start backend') {
        //             steps {
        //                 timeout(time: 5, unit: 'MINUTES') {
        //                     sh 'npm run start:dev'
        //                 }
        //             }
        //         }
        //         stage('Run tests') {
        //             steps {
        //                 echo 'Waiting for backend to start...'
        //                 sleep(time: 1, unit: 'MINUTES')
        //                 sh 'npm run testUnit'
        //             }
        //         }
        //     }
        // }
    }
    post {
        always {
            sh 'docker compose -f docker-composeRunTest.yml down --remove-orphans -v'
            sh 'docker compose ps'
        }
    }
}
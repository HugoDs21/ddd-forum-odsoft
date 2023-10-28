#!/usr/bin/env groovy

pipeline {
    agent any
    tools {
        nodejs 'Node 12.22.12'
    }
    stages {
        stage('Verify installations') {
            steps {
                sh '''
                    docker version
                    docker compose version
                    nvm ls
                    gradle -v
                '''
            }
        }

        // stage('Prepare environment') {
        //     steps {
        //         sh 'cp .env.template .env'
        //     }
        // }

        stage('Build') {
            steps {
                sh 'gradle build'
            }
        }

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

        stage('Run tests') {
            steps {
                echo 'Waiting for backend to start...'
                sleep(time: 1, unit: 'MINUTES')
                sh 'npm run testUnit'
            }
        }
    }
    post {
        always {
            sh 'docker compose -f docker-composeRunTest.yml down --remove-orphans -v'
            sh 'docker compose ps'
        }

        success {
            echo 'Tests passed'
            // junit 'test-report.html'
        }
    }
}

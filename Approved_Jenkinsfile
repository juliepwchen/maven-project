pipeline {
    agent any
    tools {
        maven 'localMaven'
    }
    stages {
        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage ('Deploy to Staging') {
            steps {
                build job: 'deploy-to-staging'
            }
        }

        stage ('Deploy to Production') {
            steps {
                timeout(time: 5, unit: 'DAYS') {
                    input message: 'Approve for Production Deployment?'
                }
                build job: 'deploy-to-prod'
            }
            post {
                success {
                    echo 'Code sucessfully deploy to Production'
                }
                failure {
                    echo 'Deployment Fail'
                }
            }
        }
    }
}
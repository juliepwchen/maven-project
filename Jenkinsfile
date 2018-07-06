pipeline {
    agent any
    tools {
        maven 'localMaven'
    }

    parameters {
        string(
            name: 'tomcat_dev',
            defaultValue: '54.67.47.138',
            description: 'Staging Server'
        )
        string(
            name: 'tomcat_prod',
            defaultValue: '13.56.139.25',
            description: 'Production Server'
        )
    }
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deployments') {
            parallel {
                stage('Deploy to Staging') {
                    steps {
                        sh "scp -i /Users/Shared/Jenkins/.ssh/tomcat-demo.pem /Users/Shared/Jenkins/Home/workspace/FullyAutomated/webapp/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }
                stage('Deploy to Production') {
                    steps {
                        sh "scp -i /Users/Shared/Jenkins/.ssh/tomcat-demo.pem /Users/Shared/Jenkins/Home/workspace/FullyAutomated/webapp/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }

            }
        }
    }
}
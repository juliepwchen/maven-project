pipeline {
    agent any
    tools {
        maven 'localMaven'
    }

    parameters {
        string(
            name: 'tomcat_dev',
            defaultValue: '13.56.210.217',
            description: 'Staging Server'
        )
        string(
            name: 'tomcat_prod',
            defaultValue: '18.144.42.67',
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
                        sh 'scp -i /Users/juliechen/Downloads/tomcat-demo.pem /Users/Shared/Jenkins/Home/workspace/FullyAutomated/webapp/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps'
                    }
                }
                stage('Deploy to Production') {
                    steps {
                        sh 'scp -i /Users/juliechen/Downloads/tomcat-demo.pem /Users/Shared/Jenkins/Home/workspace/FullyAutomated/webapp/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps'
                    }
                }

            }
        }
    }
}
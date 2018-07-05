pipeline {
    agent any
    stages {
        stage ('Build') {
            steps {
                sh 'mvn clean packagePipelineAsCode'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
            
        }
        

    }
}
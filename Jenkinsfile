pipeline {
    agent any

    tools {
    maven 'maven'
    jdk 'java8'
    }

    stages {
        stage ('init') {
            steps {
                sh '''
                echo "PATH = ${PATH}"
                echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('buid') {
            steps {
                sh '''
                mvn clean package checkstyle:checkstyle
            '''
            }
            
            post {
                success {
                    archiveArtifacts '**/*.war'
                    junit '**/target/surefire-reports/*.xml'
                    checkstyle canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
                }
            }
        }

        stage ('deploy-to-stag') {
            steps {
                build 'deploy-to-staging'
            }
        }

        stage ('Deploy to Production'){
            steps{
                timeout(2){
                    input message:'Approve PRODUCTION Deployment?'
                }

                build job: 'deploy-to-prod'
            }

        post {
            success {
                echo 'Code deployed to Production.'
            }

            failure {
                echo ' Deployment failed.'
            }           
        }
    }
}


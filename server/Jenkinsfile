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
stage ('deploy_to_stag') {
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
}



	
Click here to Reply, Reply to all or Forward
5.88 GB (39%) of 15 GB used
Manage
Terms - Privacy
Last account activity: 0 minutes ago
Currently being used in 1 other location  Details
	
	

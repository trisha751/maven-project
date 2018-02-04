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

failure {
echo ' Deployment failed.'
}
}
}

}
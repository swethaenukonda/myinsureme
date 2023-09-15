pipeline {
  agent any 
  tools {
     maven 'M2_HOME'
        }
stages {
     stage('Git Checkout') {
       steps {
         git 'https://github.com/swethaenukonda/myinsureme.git'
             }
        }
stage('Build Package') {
       steps {
         sh 'mvn clean package'
       }
}
   stage('Publish HTML Reports') {
       steps {
         publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/Insureme/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
             }
         }
   stage('Create Docker image of App') {
       steps {
         sh 'docker build -t swethamba859/insureme-app:1.0 .'
             }
     }
stage('Docker Image Push') {
       steps {
         withCredentials([string(credentialsId: 'docker-hub2', variable: ''), usernamePassword(credentialsId: 'docker1-hub', passwordVariable: 'dockerhub-password', usernameVariable: 'dockerhub-user')]) {
         sh 'docker login -u ${dockerhub-user} -p ${dockerhub-password}'
       }
         sh 'docker push swethamba859/insureme-app:1.0'

}
}

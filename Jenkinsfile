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
      withCredentials([usernamePassword(credentialsId: 'docker1hub', passwordVariable: 'docker_password', usernameVariable: 'docker_user')]) {
         sh 'docker login -u ${docker_user} -p ${docker_password}'
       }
         sh 'docker push swethamba859/insureme-app:1.0'

}
}
  stage('Application Deploy-container') {
          steps {
            
          ansiblePlaybook credentialsId: 'SSH-key', disableHostKeyChecking: true, installation: 'ansible', inventory: 'prod.inventory', playbook: 'deploy.yml'
}
}
}
}

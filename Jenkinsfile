pipeline {
  environment {
    dockerimagename = "dxcuong206/dotnet-test"
    dockerImage = ""
  }
  agent none
  stages {
    stage('Checkout Source') {
      agent { label 'agent1'}
      steps {
        git branch: 'master', credentialsId: 'GitHubCred', url: 'https://github.com/Cuongdx77/Dotnet-Test.git'
      }
    }
    stage('Sonarqube') {
      agent { label 'agent2'}
      steps {
                sh 'dotnet --info'
            }
     } 
    stage("Quality gate") {
      agent any
      steps {
         waitForQualityGate abortPipeline: false, credentialsId: 'Cred-Sonarqube'
          }
      }
    stage('Build image') {
      agent { label 'agent1'}
      steps {
         sh "docker build -f Dockerfile -t dotnet-test ."
            }      
      }
   }
}

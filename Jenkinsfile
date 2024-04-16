pipeline {
  environment {
    dockerimagename = "dxcuong206/dotnet-test"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'master', credentialsId: 'GitHubCred', url: 'https://github.com/Cuongdx77/Dotnet-Test.git'
      }
    }
    stage('Sonarqube') {
      agent {
        dockerfile {
            filename 'Dockerfile-sonar'
          }
      steps {
               sh 'dotnet --info'
            }
       }
     } 
    stage("Quality gate") {
      steps {
         waitForQualityGate abortPipeline: false, credentialsId: 'Cred-Sonarqube'
          }
      }
    stage('Build image') {
      steps {
         sh "docker build -f Dockerfile -t dotnet-test ."
            }      
      }
   }
}

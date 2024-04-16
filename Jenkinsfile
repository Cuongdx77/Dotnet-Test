pipeline {
  environment {
    dockerimagename = "dxcuong206/dotnet-test"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      agent any
      steps {
        git branch: 'master', credentialsId: 'GitHubCred', url: 'https://github.com/Cuongdx77/Dotnet-Test.git'
      }
    }
    stage('Sonarqube') {
      agent {
        dockerfile {
            filename 'Dockerfile-sonar'
          }
       }
     } 
    stage("Quality gate") {
      agent any
      steps {
         waitForQualityGate abortPipeline: false, credentialsId: 'Cred-Sonarqube'
          }
      }
    stage('Build image') {
      agent any
      steps {
         sh "docker build -f Dockerfile -t dotnet-test ."
            }      
      }
   }
}

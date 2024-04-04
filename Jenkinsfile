pipeline {
  environment {
    dockerimagename = "dxcuong206/dotnet-test"
    dockerImage = ""
  }
  agent any
  stages {
    stage('Checkout Source') {
      steps {
        git branch: 'master', credentialsId: 'GitHubCred', url: 'https://github.com/Cuongdx77/Project_website.git'
      }
    }
    stage('Test backend with dotnet') {
      steps {
         sh "docker build -f Dockerfile-sonar -t dotnet-sonarscan --rm ."
            }
        } 
    stage('Build image') {
      steps {
         sh "docker build -f Dockerfile -t dotnet-test --rm ."
            }      
      }
   }
}

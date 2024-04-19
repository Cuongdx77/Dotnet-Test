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
      agent { label 'agent3'}
      steps {
           withSonarQubeEnv('Sonarqube server connection'){
                sh 'cd /root/ETicaretAPI \
                 && docker build -f Dockerfile-sonar -t dotnet-sonarscan --rm .'
           }
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

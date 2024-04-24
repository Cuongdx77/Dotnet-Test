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
        withCredentials(credentialsId : 'Sonarqube_Cred' ) {
           withSonarQubeEnv('Sonarqube server connection'){
                sh 'cd /root/ETicaretAPI'
                sh 'docker build -f Dockerfile-sonar -t dotnet-sonarscan:02 --rm .'
                timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate 'true'
             }
           }
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

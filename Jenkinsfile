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
            sh 'cd /root/ETicaretAPI'
            sh 'docker build -f Dockerfile-sonar -t dotnet-sonarscan:02 --rm .'
        }
        script {
            def qualitygate = waitForQualityGate()
            if (qualitygate.status != "OK") {
                error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
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

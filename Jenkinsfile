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
    stage('Test backend with dotnet') {
      steps {
         sh "docker build -f Dockerfile-sonar -t dotnet-sonarscan:02 --rm ."
         timeout(time: 10, unit: 'MINUTES') {
         script {
           def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                      error "Pipeline aborted due to a quality gate failure: ${qg.status}"
                  }
              }
            }
          }
        } 
    stage('Build image') {
      steps {
         sh "docker build -f Dockerfile -t dotnet-test --rm ."
            }      
      }
   }
}

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
        environment {
                scannerHome = tool 'sonarqube_scanner'
            }
     steps {
              withSonarQubeEnv('Sonarqube_server'){
                  sh 'mvn clean package sonar:sonar'
                  sh 'cd /root/ETicaretAPI'
                  sh 'docker build -f Dockerfile-sonar -t dotnet-sonarscan:02 --rm .'
            }
          }
      }
    stage("Quality Gate") {
      agent { label 'agent3'}
      steps {
        timeout(time: 1, unit: 'HOURS') {
          waitForQualityGate abortPipeline: true
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

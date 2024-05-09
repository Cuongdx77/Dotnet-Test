pipeline {
  environment {
    dockerimagename = "dxcuong206/dotnet-test"
    dockerImage = ""
  }
  agent none
  tools {
    msbuild: 'sonarqube_scanner' 
  }
  stages {
    stage('Checkout Source') {
      agent { label 'agent1'}
      steps {
        git branch: 'master', credentialsId: 'GitHubCred', url: 'https://github.com/Cuongdx77/Dotnet-Test.git'
      }
    }

    stage('SonarQube Analysis') {
      agent { label 'agent3'}
      steps {
        script {
          def scannerHome = tool 'sonarqube_scanner'
          withSonarQubeEnv('Sonarqube_server') {
            bat "dotnet ${scannerHome}\\SonarScanner.MSBuild.dll begin /k:\"dotnet-test\""
            bat "dotnet build"
            bat "dotnet ${scannerHome}\\SonarScanner.MSBuild.dll end"
          }
        }
      }
    }

    stage("Quality Gate") {
      agent { label 'agent3'}
      steps {
        timeout(time: 1, unit: 'HOURS') {
          waitForQualityGate abortPipeline: true, credentialsId: 'Sonarqube_Cred'
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

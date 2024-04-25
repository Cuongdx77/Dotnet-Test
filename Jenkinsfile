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
                  SonarScanner.MSBuild.exe begin /k:"testsonarqube" /d:sonar.host.url="http://10.26.2.215:9000/" /d:sonar.login="sqa_c1155d4ddbbf2cf5a084a2b9b2ebb40784080a2f"
                  MSBuild.exe ETicaretAPI/src/ETicaretAPI.sln /t:Rebuild
                  SonarScanner.MSBuild.exe end /d:sonar.login="sqa_c1155d4ddbbf2cf5a084a2b9b2ebb40784080a2f"
                  #sh 'cd /root/ETicaretAPI'
                  #sh 'docker build -f Dockerfile-sonar -t dotnet-sonarscan:02 --rm .'
              }
            }
          }
    stage("Quality Gate") {
      agent { label 'agent3'}
      steps {
        timeout(time: 1, unit: 'HOURS') {
          script {
            def qualitygate = waitForQualityGate()
            if (qualitygate.status != "OK") {
              error "Pipeline aborted due to quality gate coverage failure: ${qualitygate.status}"
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

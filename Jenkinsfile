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
                 && dotnet tool install --global dotnet-sonarscanner \
                 && dotnet tool install --global coverlet.console \
                 && dotnet sonarscanner begin /k:"test-sonarqube" /d:sonar.host.url="http://10.26.2.215:9000"  /d:sonar.token="sqp_d23765f8669b627f6488af88ba6648beb7aeff57" \
                 && dotnet build \
                 && dotnet sonarscanner end /d:sonar.token="sqp_d23765f8669b627f6488af88ba6648beb7aeff57"'
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

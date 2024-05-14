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
                  sh 'cd /root/ETicaretAPI'
                  sh 'docker build -f Dockerfile-sonar -t dotnet-sonarscan:02 --rm .'
            }
          }
      }
  stage("Quality Gate") {
    agent { label 'agent3'}
    steps {
        script {
            def response = sh(script: 'curl -u "sqa_1930a831282b897e091d3074560eb2ef2e0bf5c8:" "10.26.2.215:9000/api/qualitygates/project_status?projectKey=test-sonarqube"', returnStdout: true).trim()
            def status = sh(script: "echo '${response}' | jq -r '.projectStatus.status'", returnStdout: true).trim()
            if ("${status}" == 'OK') { 
                        currentBuild.result = 'ABORTED' 
                        error('Job Aborted') 
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

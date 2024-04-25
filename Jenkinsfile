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
                script {
                    scannerHome = tool 'SonarScanner for MSBuild 5.15.1.88158 - .NET Fwk 4.6'
                }
                
                withSonarQubeEnv('Sonarqube server connection') {
                    sh 'cd /root/ETicaretAPI'
                    sh 'dotnet sonarscanner begin /k:"testsonarqube" /d:sonar.host.url="http://10.26.2.215:9000"  /d:sonar.token="sqa_c1155d4ddbbf2cf5a084a2b9b2ebb40784080a2f"'
                    sh "dotnet build"
                    sh 'dotnet sonarscanner end /d:sonar.token="sqa_c1155d4ddbbf2cf5a084a2b9b2ebb40784080a2f"'
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

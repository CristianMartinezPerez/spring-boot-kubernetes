pipeline {
    agent any

    tools {
        maven 'Maven'
    }
    stages {
        stage ('Package') {
            steps {
                 bat 'mvn clean package -Dmaven.test.skip=true'
            }
        }

        stage('SonarQube analysis') {
           steps{
                script {
                    def scannerHome = tool 'sonar-scanner';//def scannerHome = tool name: 'SonarQube Scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv(installationName: 'SonarQube') {
                      bat "${scannerHome}\\bin\\sonar-scanner -Dsonar.projectKey=DevSecOps-Tarea4 -Dsonar.sources=target/ -Dsonar.host.url=http:/localhost:9000 -Dsonar.login=6e4c9e0826331de0946590d766e4d1bc62352271"
                      //sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=Maven -Dsonar.host.url=http://172.18.0.3:9000 -Dsonar.login=3669ed23eaa7500048d3d0a87a43669d3db349af -Dsonar.dependencyCheck.jsonReportPath=target/dependency-check-report.json -Dsonar.dependencyCheck.xmlReportPath=target/dependency-check-report.xml -Dsonar.dependencyCheck.htmlReportPath=target/dependency-check-report.html"

                    }
                }
           }
        }

    }
}
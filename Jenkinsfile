pipeline {
    agent any

    tools {
        maven 'Maven'
    }
    stages {
	    stage ('Compile') {
            tools {
          	    jdk 'JDK8'
              }
            steps {
                 bat 'mvn clean compile -e'
            }
        }
      stage ('Jar') {
            tools {
          	    jdk 'JDK8'
              }
            steps {
                 bat 'mvn clean package'
            }
      }

        stage('SAST') {
           tools {
          	    jdk 'JDK11'
              }
           steps{
                script {
                    def scannerHome = tool 'sonar-scanner';
                    withSonarQubeEnv(installationName: 'SonarQube') {
                      bat "${scannerHome}\\bin\\sonar-scanner -Dsonar.projectKey=DevSecOps-Tarea45 -Dsonar.sources=target/ -Dsonar.host.url=http:/localhost:9000 -Dsonar.login=6e4c9e0826331de0946590d766e4d1bc62352271"
                    }
                }
           }
        }

    }
}
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

        stage ('SCA') {
            steps {
                 bat 'mvn org.owasp:dependency-check-maven:check'
                dependencyCheckPublisher failedNewCritical: 5, failedTotalCritical: 10, pattern: 'terget/dad.xml', unstableNewCritical: 3, unstableTotalCritical: 5
            }
        }

        stage('Scan Docker'){
                steps{
                    figlet 'Scan Docker (Version windows cmd)'
                    script{
                        /* 
                        bat 'docker build -t spring-boot-k8s:v1 .'
                        bat 'docker run --rm -w /root/.cache/ -v "%cd%:/root/.cache/" aquasec/trivy spring-boot-k8s:v1'
                        bat 'docker rmi --force aquasec/trivy'
                        bat 'docker images | findstr spring-boot > contenedor.txt'
                        bat 'for /f "tokens=3" %a in (contenedor.txt) do docker image rm %a'
                        bat 'del contenedor.txt'
                        */
                        
                        bat 'docker run --rm -w /root/.cache/ -v "%cd%:/root/.cache/" aquasec/trivy --format template --template "@contrib/html.tpl" -o scan.html openjdk:8-jdk-alpine'
                        bat 'docker rmi --force aquasec/trivy'

                    }
                }
            }

        stage('DAST'){            
				steps{
                  script{
                          bat 'docker rmi -f owasp/zap2docker-stable'
                          bat 'docker pull owasp/zap2docker-stable'
                          bat 'docker run -w /zap/wrk -v "%cd%:/zap/wrk" -t owasp/zap2docker-stable zap-full-scan.py -t http://zero.webappsecurity.com/ -g gen.conf -r testreport.html'
                  }
	            }
            } 
            


    }
}
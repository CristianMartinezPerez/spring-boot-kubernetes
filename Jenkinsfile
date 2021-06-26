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

        stage('DAST'){            
					steps{
                  script{
                      try {
                          bat 'docker rmi -f owasp/zap2docker-stable'
                          bat 'docker pull owasp/zap2docker-stable'
                          bat 'docker run -w /zap/wrk -v "%cd%:/zap/wrk" -t owasp/zap2docker-stable zap-full-scan.py -t http://zero.webappsecurity.com/ -g gen.conf -r testreport.html'
                        } catch (Exception e) {
                            echo 'Error en DAST: ' + e.toString()
                            bat  'echo Continua Ejecucion e reporte siguiente'
                        }


                  }
	              }
            } 
            
             stage('Scan Docker'){
                    			steps{
                    			    figlet 'Scan Docker'
                    		        script{
                                        bat 'docker run --rm -w /root/.cache/ -v "%cd%:/root/.cache/" aquasec/trivy python:3.4-alpine'
                                        bat 'docker rmi --force aquasec/trivy'
    
                    		        }
                    			}
                    		}

    }
}
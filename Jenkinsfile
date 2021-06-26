pipeline {
    agent any
    stages {
      stage ('Build') {
            steps {
              echo '========================================='
              echo '                BUILDING '
              echo '========================================='     
            }
        }
        stage ('Test') {
            steps {
                echo '========================================='
                echo '                TESTING '
                echo '========================================='
            }
        }
        stage ('Deploy') {
            steps {
                echo '========================================='
                echo '                DEPLOYING... '
                echo '========================================='
            }
        }
        stage ('SCA') {
            steps {
                sh 'mvn org.owasp:dependency check-maven:check'
                dependencyCheckPublisher failedNewCritical: 5, failedTotalCritical: 10, pattern: 'terget/dad.xml', unstableNewCritical: 3, unstableTotalCritical: 5
            }
        }
        stage ('SonarQube analysis') {
           steps{
                script {
                    def SonarScanner = tool 'SonarQube Scanner';
                    withSonarQubeEnv('Sonar Server') {
                      sh "${SonarScanner}/bin/sonar-scanner -Dsonar.projectKey=tareausach -Dsonar.java.binaries=target/ -Dsonar.host.url=http://0.0.0.0:9000 -Dsonar.login=324806140df35a6fe38ff92cef4b99446f941f36"
                  }
                }
            }
        //stage ('ZAP'){
          //  steps {
             //   figlet 'Owasp Zap DAST'
               // script{
                 //   env.DOCKER = tool "Docker"
                   // env.DOCKER_EXEC = "${DOCKER}/bin/docker"
                   // sh '${DOCKER_EXEC} rm -f zap2'
                   // sh '${DOCKER_EXEC} pull owasp/zap2docker-stable'
                   // sh '${DOCKER_EXEC} run --add-host="localhost:0.0.0.0" --rm -e LC_ALL=C.UTF-8 -e LANG=C.UTF-8 --name zap2 -u zap -p 8090:8080 -d owasp/zap2docker-stable zap.sh -daemon -port 8080 -host 0.0.0.0 -config api.disablekey=true'
                   // sh '${DOCKER_EXEC} run --add-host="localhost:0.0.0.0" -v /home/kali/DevSecOps/zap:/zap/wrk/:rw --rm -i owasp/zap2docker-stable zap-full-scan.py -t "http://demo.testfire.net/" -I -r zap_baseline_report2.html -l PASS'
               // }
           // }
        }
    }
}

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
        stage ('SonarQube analysis') {
           steps{
                script {
                    def SonarScanner = tool "SonarScanner";
                    withSonarQubeEnv('Sonar Server') {
                      sh "${SonarScanner}/bin/sonar-scanner -Dsonar.projectKey=ayudantia -Dsonar.java.binaries=. -Dsonar.host.url=http://172.18.0.2:9000 -Dsonar.login=aacf352390357dd383acce70b1e7c087e35c8cb3"
                  }
                }
            }
        }
        stage ('SCA') {
            steps {
                sh 'mvn org.owasp:dependency-check-maven:aggregate'
                dependencyCheckPublisher pattern: '${WORKSPACE}/target/dependency-check-report.html'
            }
        }
        //stage ('ZAP'){
            //steps {
                //figlet 'Owasp Zap DAST'   //${DOCKER_EXE}
              //  script{
                   // env.DOCKER = tool "Docker"
                  //  env.DOCKER = "${DOCKER}/bin/docker"
                  //  sh '${DOCKER} rm -f zap2'
                 //   sh '${DOCKER} pull owasp/zap2docker-stable'
                   // sh '${DOCKER} run --add-host="localhost:127.0.0.1" --rm -e LC_ALL=C.UTF-8 -e LANG=C.UTF-8 --name zap2 -u zap -p 8090:8080 -d owasp/zap2docker-stable zap.sh -daemon -port 8080 -host 0.0.0.0 -config api.disablekey=true'
                  //  sh '${DOCKER} run --add-host="localhost:127.0.0.1" -v /var/jenkins_home/ZAP_2.10.0:/zap/wrk/:rw --rm -i owasp/zap2docker-stable zap-full-scan.py -t "http://demo.testfire.net/" -I -r zap_baseline_report2.html -l PASS'
            // }
          //  }
        //}
        stage('Scan Docker'){
            steps{
                // figlet 'Scan Docker'
                script{
                    env.DOCKER = tool "Docker"
                    env.DOCKER_EXEC = "${DOCKER}/bin/docker"
                    sh '''
                    ${DOCKER} run --rm -v $(pwd):/root/.cache/ aquasec/trivy python:3.4-alpine
                    '''
                    sh '${DOCKER} rmi aquasec/trivy'
                }
            }
        }   
    }
}

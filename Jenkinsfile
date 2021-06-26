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
        stage ('ZAP'){
            steps {
                figlet 'Owasp Zap DAST'
                script{
                    env.DOCKER = tool "Docker"
                    env.DOCKER_EXEC = "${DOCKER}/bin/docker"
                    env.TARGET ='https://demo.testfire.net/'

                    sh '${DOCKER_EXEC} ps'
                }
            }
        }
    }
}

pipeline {
    agent none

    stages {
       
        
       stage ('Check-Git-Secrettss') {
         steps {
           sh 'rm trufflehog || true'
           sh 'sudo docker run  --log-driver json-file --log-opt max-size=1g --log-opt max-file=2 gesellix/trufflehog https://github.com/dhananjaybhakte/Devsecops.git > trufflehog'
           sh 'sudo cat /var/lib/jenkins/workspace/Devsecopsnew/trufflehog'
         }
       }
       stage("Test and Sonar Scan") {
            agent any
            steps {
                sh 'npm install'
                sh 'npm test'
                sh 'npm run sonar'
            }
        }
        
        stage('Dependency scaner') {
          steps {
            dir(path: 'angular-frontend') {
              sh 'npm audit'
              
            }

          }
        }
        
        stage("build and deploy the project") {
            agent {
                 docker {
                  image 'node'
                  args '-p 3000:3000'
                }
            }
            stages {
               stage("build") {
                   steps {
                       sh 'npm install'
                   }
               }
                stage('Test') {
                    steps {
                        sh 'sh ./jenkins/scripts/test.sh'
                    }
                }
               stage('Deliver') { 
                steps {
                    sh 'sh ./jenkins/scripts/deliver.sh' 
                    input message: 'Finished using the web site? (Click "Proceed" to continue)' 
                    sh 'sh ./jenkins/scripts/kill.sh' 
                }
            } 
            }
        }


    }
           environment {
            CI = 'true'
            HOME = '.'
          }
}

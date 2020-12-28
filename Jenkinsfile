pipeline {
  agent any
  
  stages {
        stage("build and test the project") {
            agent {
              docker {
                image 'node'
                args '-p 3000:3000'
              }

            }
            stages {
                   stage('Build') {
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
                        input 'Finished using the web site? (Click "Proceed" to continue)'
                        sh 'sh ./jenkins/scripts/kill.sh'
                      }
                    }
            }
        }
  

     stage("Security") {
         agent none
          stage('sonar scan') {
                      steps {
                        sh 'npm install'
                      }
                      steps {
                        sh 'npm run sonar'
                      }
            }
     }

  }
  environment {
    CI = 'true'
    HOME = '.'
  }
}

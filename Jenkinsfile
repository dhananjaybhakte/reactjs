pipeline {
    agent none

    stages {
        
       stage("Test and Sonar Scan") {
            agent any
            steps {
                sh 'npm install'
                sh 'npm test'
                sh 'npm run sonar'
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
               stage('Deliver') { 
                steps {
                    sh './jenkins/scripts/deliver.sh' 
                    input message: 'Finished using the web site? (Click "Proceed" to continue)' 
                    sh './jenkins/scripts/kill.sh' 
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

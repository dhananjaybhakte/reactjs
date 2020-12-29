pipeline {
        agent {
         docker {
          image 'node'
          args '-p 3000:3000'
        }
    }

    stages {
        
       stage("Test and Sonar Scan") {
            agent any
            steps {
                echo 'npm install'
                echo 'npm test'
                echo 'npm run sonar'
            }
        }
        stage("build and deploy the project") {
            stages {
               stage("build") {
                   steps {
                       echo 'npm install'
                   }
               }
                stage('Test') {
                    steps {
                        sh './jenkins/scripts/test.sh'
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
          }
}

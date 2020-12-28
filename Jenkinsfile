pipeline {
    agent none

    stages {
        stage("build and test the project") {
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
               stage("test") {
                   steps {
                       sh 'sh ./jenkins/scripts/test.sh'
                   }
               }
            }
            post {
                success {
                    stash name: "artifacts", includes: "artifacts/**/*"
                }
            }
        }

        stage("deploy the artifacts if a user confirms") {
            input {
                message "Should we deploy the project?"
            }
            agent {
                docker "our-deploy-tools-image"
            }
            steps {
                sh "./deploy.sh"
            }
        }
    }
   environment {
    CI = 'true'
    HOME = '.'
  }
}

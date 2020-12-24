pipeline {
  agent {
    docker {
      image 'node:6-alpine'
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
      parallel {
        stage('Test') {
          steps {
            sh 'sh ./jenkins/scripts/test.sh'
          }
        }

        stage('sonar scanner') {
          steps {
            sh 'npm run sonar'
          }
        }

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
  environment {
    CI = 'true'
  }
}
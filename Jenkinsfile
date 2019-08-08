pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('stage 01') {
      parallel {
        stage('stage 01-01') {
          steps {
            sh 'echo \'exec at stage 01-01\''
          }
        }
        stage('stage 01-02') {
          steps {
            sh 'echo \'exec at stage 01-02\''
          }
        }
      }
    }
    stage('stage 02-01') {
      parallel {
        stage('stage 02-01') {
          steps {
            echo 'exec at stage 02-01'
          }
        }
        stage('stage 02-02') {
          steps {
            echo 'exec at stage 02-02'
          }
        }
      }
    }
    stage('confirm') {
      steps {
        input 'OK?'
      }
    }
  }
}
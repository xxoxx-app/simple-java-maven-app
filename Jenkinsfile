pipeline {
  agent {
    docker {
      image 'maven:3.6.1'
    }

  }
  stages {
    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
      }
    }
  }
}
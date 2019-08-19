pipeline {
    agent {
        label 'master'
        docker {
            image 'maven:3-alpine' 
            args '-v /opt/maven_localdata:/root/.m2' 
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

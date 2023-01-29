pipeline {
  agent any
  tools {
    maven 'maven-3.6.3' 
  }
  stages {
    stage ('Build') {
      steps {
        sh 'mvn release:clean release:prepare release:perform'
      }
    }
    stage ('Deploy') {
      steps {
        script {
          echo "<please implement deploy stage...>"
        }
      }
    }
  }
}

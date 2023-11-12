pipeline {
  agent any
  tools {
    maven 'maven' 
  }
  stages {
    stage ('Build') {
      steps {
        sh """
        git config --global user.email "thermaleagle@gmail.com"
        git config --global user.name "Thermal Eagle"
        """
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

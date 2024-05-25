pipeline {
  agent any
  tools {
    maven 'maven' 
  }
  environment {
      PATH = "${env.PATH}:${GPGPATH}"
  }
  stages {
    stage ('Build') {
      steps {
        sh """
        git config --global user.email "thermaleagle@gmail.com"
        git config --global user.name "Thermal Eagle"
        """
        configFileProvider(
          [configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
            sh 'ls -l ${GPGPATH}'
            sh '${GPGPATH}/gpg --version'
            sh 'echo $PATH'
            sh 'mvn -s $MAVEN_SETTINGS release:clean release:prepare release:perform'
          }
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

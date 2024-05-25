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
            sh 'ls -l /opt/homebrew/Cellar/gnupg/2.4.3/bin/gpg'
            sh '/opt/homebrew/Cellar/gnupg/2.4.3/bin/gpg --version'
            sh 'echo $PATH'
            
            sh '${GPGPATH} --version'
            sh 'mvn -s $MAVEN_SETTINGS release:clean release:prepare release:perform -Dgpg.executable=${GPGPATH} -Drelease.arguments="-Dgpg.executable=${GPGPATH}"'
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

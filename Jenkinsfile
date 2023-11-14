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
        configFileProvider(
          [configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
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

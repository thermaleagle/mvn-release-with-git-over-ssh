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
        configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
            //sh 'mvn -s $MAVEN_SETTINGS release:clean release:prepare release:perform'

            withCredentials([string(credentialsId: 'gpg-passphrase', variable: 'GPG_PASSPHRASE')]) {
              sh '''
                  export GPG_TTY=true
                  mvn -s $MAVEN_SETTINGS release:clean release:prepare release:perform \
                      -Dgpg.executable=${GPGPATH}/gpg \
                      -Dgpg.useagent=true \
                      -Dgpg.batch=true \
                      -Dgpg.passphrase=${GPG_PASSPHRASE} \
                      -Dgpg.pinentry-mode=loopback \
                      -Drelease.arguments="-Dgpg.executable=${GPGPATH}/gpg -Dgpg.useagent=true -Dgpg.batch=true -Dgpg.passphrase=${GPG_PASSPHRASE} -Dgpg.pinentry-mode=loopback"
              '''
            }
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

pipeline {
  agent any
  tools {
    maven 'maven' 
  }
  environment {
      PATH = "${env.PATH}:${GPGPATH}"
      GPG_SECRET_KEY = credentials('gpg-secret-key') // Use the ID of your stored GPG key
  }
  stages {
    stage('Setup GPG') {
        steps {
            script {
                // Create the GPG directory and import the key
                sh 'mkdir -p /var/jenkins_home/.gnupg'
                writeFile file: '/var/jenkins_home/.gnupg/private-key.asc', text: GPG_SECRET_KEY
                sh 'gpg --import /var/jenkins_home/.gnupg/private-key.asc'
            }
        }
    }
    stage ('Build') {
      steps {
        sh """
        git config --global user.email "thermaleagle@gmail.com"
        git config --global user.name "Thermal Eagle"
        """
        configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
            sh 'mvn -s $MAVEN_SETTINGS release:clean release:prepare release:perform'

            // withCredentials([string(credentialsId: 'gpg-passphrase', variable: 'GPG_PASSPHRASE')]) {
            //   sh '''
            //       export GPG_TTY=true
            //       mvn -s $MAVEN_SETTINGS release:clean release:prepare release:perform \
            //           -Dgpg.executable=${GPGPATH}/gpg \
            //           -Dgpg.useagent=true \
            //           -Dgpg.batch=true \
            //           -Dgpg.passphrase=${GPG_PASSPHRASE} \
            //           -Dgpg.pinentry-mode=loopback \
            //           -Drelease.arguments="-Dgpg.executable=${GPGPATH}/gpg -Dgpg.useagent=true -Dgpg.batch=true -Dgpg.passphrase=${GPG_PASSPHRASE} -Dgpg.pinentry-mode=loopback"
            //   '''
            // }
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

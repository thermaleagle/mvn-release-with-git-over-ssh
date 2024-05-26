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
                    // Create the GPG directory
                    sh 'mkdir -p /var/jenkins_home/.gnupg'
                    
                    // Write the GPG key file from Jenkins credentials
                    writeFile file: '/var/jenkins_home/.gnupg/private-key.asc', text: GPG_SECRET_KEY

                    // Print the contents of the key file for debugging
                    echo GPG_SECRET_KEY // Be careful, this exposes the key in logs, use only for debugging

                    sh 'chmod 600 /var/jenkins_home/.gnupg/private-key.asc'

                    sh 'ls -lrt /var/jenkins_home/.gnupg/private-key.asc'

                    // Print the contents of the key file for debugging
                    sh 'cat /var/jenkins_home/.gnupg/private-key.asc'
              
                    // Import the GPG key
                    sh 'gpg --import /var/jenkins_home/.gnupg/private-key.asc'
                    
                    // List the imported keys to verify
                    sh 'gpg --list-secret-keys'
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

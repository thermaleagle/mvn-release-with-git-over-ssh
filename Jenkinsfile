pipeline {
  agent any
  tools {
    maven 'maven' 
  }
  environment {
      PATH = "${env.PATH}:${GPGPATH}"        
      GIT_SSH_KEY = credentials('git-ssh-key') // Use the correct credentials ID for the SSH key
      KNOWN_HOSTS = credentials('known-hosts') // Use the ID for the known hosts
      GPG_SECRET_KEY = credentials('gpg-secret-key') // Use the ID of your stored GPG key
      GPG_PASSPHRASE = credentials('gpg-passphrase') // Use the ID for the GPG passphrase
  }
  stages {
    stage('Setup SSH') {
        steps {
            withCredentials([sshUserPrivateKey(credentialsId: 'git-ssh-key', keyFileVariable: 'SSH_KEY_FILE', passphraseVariable: '', usernameVariable: 'SSH_USER')]) {
                script {
                    // Create the SSH directory
                    sh 'mkdir -p /var/jenkins_home/.ssh'
                    
                    // Add the known hosts to avoid host key verification failures
                    withCredentials([file(variable: 'KNOWN_HOSTS_FILE', credentialsId: 'known-hosts')]) {
                        sh 'cat $KNOWN_HOSTS_FILE > /var/jenkins_home/.ssh/known_hosts'
                    }

                    // Start SSH agent and add the SSH key
                    sh '''
                        eval $(ssh-agent -s)
                        ssh-add $SSH_KEY_FILE
                    '''
                }
            }
        }
    }
    stage('Setup GPG') {
        steps {
            script {
                // Create the GPG directory
                sh 'mkdir -p /var/jenkins_home/.gnupg'
                
                // Print the environment variable for debugging
                echo "GPG_SECRET_KEY: ${GPG_SECRET_KEY}"

                // Use the temporary secret file directly for GPG import
                withCredentials([file(variable: 'GPG_KEY_FILE', credentialsId: 'gpg-secret-key')]) {
                    // Print the path of the temporary file
                    sh 'echo $GPG_KEY_FILE'

                    // Import the GPG key
                    sh 'gpg --batch --import $GPG_KEY_FILE'
                    
                    // List the imported keys to verify
                    sh 'gpg --list-secret-keys'
                }
            }
        }
    }
    stage ('Build') {
      steps {
        withEnv(["GPG_PASSPHRASE=${GPG_PASSPHRASE}"]) {
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
    }
    stage ('Deploy') {
      steps {
        script {
          echo "<please implement deploy stage...>"
        }
      }
    }
    stage('Cleanup') {
        steps {
            script {
                // Remove the temporary files
                sh 'rm -f /var/jenkins_home/.ssh/id_rsa'
                sh 'rm -f /var/jenkins_home/.gnupg/private-key.asc'
            }
        }
    }
  }
}

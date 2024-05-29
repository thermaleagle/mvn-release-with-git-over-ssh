pipeline {
  agent any
  tools {
    maven 'maven' 
  }
  stages {
    stage('Setup SSH') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'github-ssh-key', keyFileVariable: 'SSH_KEY_FILE')]) {
          script {
            // Create the SSH directory
            sh 'mkdir -p /var/jenkins_home/.ssh'
            
            // Copy the SSH key to the .ssh directory
            sh 'cp $SSH_KEY_FILE /var/jenkins_home/.ssh/id_ed25519'
            
            // Set the correct permissions for the SSH key
            sh 'chmod 600 /var/jenkins_home/.ssh/id_ed25519'

            // Add the known hosts to avoid host key verification failures
            withCredentials([file(variable: 'KNOWN_HOSTS_FILE', credentialsId: 'known-hosts')]) {
                sh 'cp $KNOWN_HOSTS_FILE /var/jenkins_home/.ssh/known_hosts'
                sh 'chmod 644 /var/jenkins_home/.ssh/known_hosts'
            }

            // Start SSH agent and add the SSH key
            sh '''
                eval $(ssh-agent -s)
                ssh-add /var/jenkins_home/.ssh/id_ed25519
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

          // Use the temporary secret file directly for GPG import
          withCredentials([file(variable: 'GPG_KEY_FILE', credentialsId: 'gpg-secret-key')]) {
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
        withCredentials([string(credentialsId: 'gpg-passphrase', variable: 'GPG_PASSPHRASE')]) {
          sh """
          git config --global user.email "thermaleagle@gmail.com"
          git config --global user.name "Thermal Eagle"
          """
          configFileProvider([configFile(fileId: 'maven-settings', variable: 'MAVEN_SETTINGS')]) {
              sh 'mvn -s $MAVEN_SETTINGS release:clean release:prepare release:perform'
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
            sh 'rm -f /var/jenkins_home/.ssh/id_ed25519'
            sh 'rm -f /var/jenkins_home/.gnupg/private-key.asc'
        }
      }
    }
  }
}

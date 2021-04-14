CODE_CHANGES = getGitChanges()
pipeline {
    agent any
    environment{
      SSH_CREDENTIALS = credentials('Application_Server_SSH')
    }
    stages {
        stage('Build') {
            when {
              expression {
                CODE_CHANGES == true
              }
            }
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('SSH transfer') {
          script {
            sshPublisher(
            continueOnError: false, failOnError: true,
            publishers: [
              sshPublisherDesc(
              configName: "Application_Server",
              verbose: true,
              transfers: [
                sshTransfer(
                sourceFiles: "target/webapp.war",
                removePrefix: "target",
                remoteDirectory: ".",
                execCommand: ""
                )
              ])
            ])
          }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
    post  {
      always{
          
      }
      success {
      
      }
      failure {
      
      }
    }
}

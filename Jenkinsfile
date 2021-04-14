pipeline {
    agent any
    environment{
      SSH_CREDENTIALS = credentials('Application_Server_SSH')
    }
    stages {
        stage('Build') {
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
          steps {
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
          }}
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}

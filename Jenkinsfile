CODE_CHANGES = getGitChanges()
pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2'
        }
    }
    environment{
      SSH_CREDENTIALS = credentials('Application_Server_SSH')
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
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

pipeline {
    agent any
    environment {
        // GOPATH = "/path/to/gopath"
        // PATH = "$PATH:$GOPATH/bin"
        GO111MODULE = "on"
        PATH = "${PATH}:${tool 'go'}/bin"
    }

    tools { go 'go' }
    stages {
        stage('Build Base Image') {
            steps {
                sh 'whoami'
                sh 'make build-base'
            }
        }
        stage('Run Tests') {
            steps {
                // sh 'go install github.com/jstemmer/go-junit-report/v2@latest'
                // sh 'go get -u github.com/jstemmer/go-junit-report'
                //sh 'export GO111MODULE=on && go install github.com/jstemmer/go-junit-report/v2@latest'
                sh 'make build-test'
                sh 'make test-unit'
            }
        }
        stage('Debugging') {
            steps {
                sh 'ls -l'
            }
        }
        stage('Publish Test Results') {
            steps {
                junit '**/TEST-*.xml'
            }
        }
        stage('Build Final Docker Image') {
            steps {
                //sh 'go install github.com/jstemmer/go-junit-report/v2@latest'
                sh 'make build'
            }
        }
        stage('Deploy to EC2') {
            steps {
                echo "HI"
                // Install Docker on the EC2 instance
                // Copy application files to the EC2 instance
                // Build and run Docker container on the EC2 instance
            }
        }
    }
    post {
        always {
            slackSend(
                color: "#00FF00",
                channel: "jenkins-notify",
                message: "${currentBuild.fullDisplayName} succeeded",
                tokenCredentialId: 'slack-token'
            )
        }
        failure {
            slackSend(
                color: "#FF0000",
                channel: "jenkins-notify",
                message: "${currentBuild.fullDisplayName} was failed",
                tokenCredentialId: 'slack-token'
            )
        }
    }
}
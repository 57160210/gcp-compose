
library 'pipeline-libraries@develop'
def agentLabel
def env
def command

node('master') {

    // define label for jenkins-slave
    stage('Check Agent') {
        agentLabel = 'PROD-CD-SLAVE'
    }
}

pipeline {
    agent {
        node {
            label agentLabel
        }
        kubernetes {
            yaml '''
                apiVersion: v1
                kind: Pod
                spec:
                containers:
                - name: maven
                    image: maven:alpine
                    command:
                    - cat
                    tty: true
                - name: busybox
                    image: busybox
                    command:
                    - cat
                    tty: true
            '''
        }
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        disableConcurrentBuilds()
    }
    
    stages {
        stage('run-maven') {
            steps {
                script {
                    container('maven') {
                        sh 'mvn -version'
                    }
                    container('busybox') {
                        sh '/bin/busybox'
                    }
                }
            }
        }
    }
}
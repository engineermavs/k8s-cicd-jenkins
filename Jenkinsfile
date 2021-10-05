@Library('shared-library') _

pipeline {
    agent any
    environment {
        DOCKER_CRED = 'dockerhub'
    }
    stages {
        stage("Checkout code") {
            steps {
                script {
                    scm()
                }
            }
        }
        stage("Build image") {
            steps {
                script {
                    step.npmBuild()
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    step.pushDocker()
                }
            }
          }        
        }
   }

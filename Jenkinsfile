@Library('shared-library') _

pipeline {
    agent any
    environment {
        DOCKER_CRED = 'dockerhub'
    }
    stages {
        stage("Checkout code") {
            steps {
                scm()
            }
        }
        stage("Build image") {
            steps {
                script {
                    npmStep.build()
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    npmStep.push()
                }
            }
          }        
        }
   }

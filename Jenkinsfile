pipeline {
    agent any
    environment {
        CREDENTIALS_ID = 'Kubernetes'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage("Build image") {
            steps {
                script {
                    myapp = docker.build("jaganthoutam/hello:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }        
        stage('Deploy to K8s') {
          steps  {
            kubernetes {
                label "Jenkins-${env.JOB_NAME}"
                yaml libraryResource('deployment.yaml')
              }
            }
        }
    }    
}

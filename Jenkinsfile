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
        stage('Deploy to GKE') {
             steps {
             sh "sed -i 's/hello:latest/hello:${env.BUILD_ID}/g' deployment.yaml"
             sshagent(['k8svagrant']) {
                 sh "scp -o StrictHostKeyChecking=no deployment.yaml vagrant@10.32.0.1:/home/vagrant/"
                 script {
                     sh "ssh vagrant@10.32.0.1 kubectl create -f deployment.yaml"
                 }
                }
             }          
          }
        }
   }

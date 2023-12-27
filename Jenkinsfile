pipeline {
    agent any

    stages {
        stage('git-checkout') {
            steps {
                
               git branch: 'main', url: 'https://github.com/Shubham-Likhar/Jenkins-Docker-Project.git'
            }
        }
        stage('docker-build') {
            steps {
                
               sh 'docker image build -t $JOB_NAME:v1.$BUILD_ID .'
               sh 'docker image tag $JOB_NAME:v1.$BUILD_ID docker7796/$JOB_NAME:v1.$BUILD_ID'
               sh 'docker image tag $JOB_NAME:v1.$BUILD_ID docker7796/$JOB_NAME:latest'
            }
        }
        stage('push-image') {
            steps {
                withCredentials([string(credentialsId: 'docker_hub', variable: 'docker_hub')]) {
                
                 sh "docker login -u docker7796 -p ${docker_hub}"
                 sh 'docker image push docker7796/$JOB_NAME:v1.$BUILD_ID'
                 sh 'docker image push docker7796/$JOB_NAME:latest'
                 } 
             }
        }
        
        stage('Deploy to container') {
            steps {
                
              sshagent(['login-vm']) {
    sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.47.83 docker rm -f docker-webapps"
    sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.47.83 docker rmi -f docker7796/docker-webapps:latest"
    sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.47.83 docker run -p 9000:80 -d --name docker-webapps docker7796/docker-webapps:latest"
   
             }
            }
        }
        
        
        
        
    }
}




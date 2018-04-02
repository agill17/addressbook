pipeline {
  agent none
  stages {
    
    stage('git clone'){
      agent { label 'master' }
      steps {
        checkout scm
      }
    }
    

    stage('Docker build') {
      agent { label 'master' }
      steps {
        sh """
             cd /tmp \
             git clone ${docker_repo} \
             cd Infrastructure-as-code/Docker/multi-stage-tomcat-app \
             sudo systemctl start docker \
             sudo docker --version
             sudo docker image build --build-arg repo=${app_repo} . --tag agill17/tomcat:${BUILD_NUMBER}
             cat /tmp/p.txt | sudo docker login --username=agill17 --password-stdin
             sudo docker image push agill17/tomcat:${BUILD_NUMBER} 
          """    
      } 
    }
    
    stage('Docker deploy') {
      agent { label 'deploy' }
      steps {
        sh "sudo yum install git -y"
        sh "sudo yum install -y yum-utils device-mapper-persistent-data lvm2"
        sh "sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo"
        sh "sudo yum install docker-ce -y"
        sh "sudo systemctl start docker"
        sh "sudo docker --version "
        sh "docker container run -dp 8080:8080 agill17/tomcat:${BUILD_NUMBER}"
      }
    }
    
  }
}

/**
pipeline {
  agent none
  stages {
    
		stage('git clone'){
      agent { label 'master' }
      steps {
        git 'https://github.com/agill17/addressbook.git'
      }
    }    
    

		stage('Docker build') {
      agent { label 'master' }
      steps {
        sh """
             cd /tmp \
             && if [ -d Infrastructure-as-Code ]; then sudo rm -rf Infrastructure-as-Code; fi \
             && git clone ${docker_repo} \
             && cd Infrastructure-as-Code/Docker/multi-stage-tomcat-app \
             && sudo systemctl start docker \
             && sudo docker --version \
             && sudo docker image build --build-arg mvn_goal="package" --build-arg repo=${app_repo} . --tag agill17/tomcat:${BUILD_NUMBER} \
             && cat /tmp/p.txt | sudo docker login --username=agill17 --password-stdin \
             && sudo docker image push agill17/tomcat:${BUILD_NUMBER}
          """
      }
    }
   	
		stage('Docker deploy') {
      agent { label 'deploy' }
      steps {
        sh """
        sudo yum install git -y \
        && sudo yum install -y yum-utils device-mapper-persistent-data lvm2 \
        && sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo \
        && sudo yum install docker-ce -y \
        && sudo systemctl start docker \
        && sudo docker --version 
        """
        sh "sudo docker container run -dp 8080:8080 agill17/tomcat:${BUILD_NUMBER}"
      }
    } 
    
  }
}
**/


@Library('github.com/agill17/Jenkins_Test@master') _

basicStandard {
    goal = "package"
    hostToDeploy = "localhost"
}



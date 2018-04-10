@Library('library') _

node {
  
  def imgTag = "agill17/tomcat:latest_jenkins"
  def contPort = "8080" 
  def hostPort = "8088"  
  def docker_hub_creds = 'docker_hub'
  def repo= 'https://github.com/agill17/addressbook.git'
  
  stage ('build and push') {
    buildImage('https://registry.hub.docker.com', docker_hub_creds, imgTag, "--build-arg repo=${repo} .")
  }
   
  stage ('run container') {
    deployContainer(imgTag, "-p ${hostPort}:${contPort}")
  }

}

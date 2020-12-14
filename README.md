# CI-CD-using-Docker
This repository will run a java application in a tomcat container using  Jenkins and Docker


# Youtube Link

https://www.youtube.com/watch?v=B1sjiq1wD_Y&feature=youtu.be

# Blog Link
https://devops4solutions.com/ci-cd-using-jenkins-and-docker-2/


Requirement:
jenkins server setup on aws ec2 :
---------------------------------
Install jenkins latest stable version with suggested plugins 

Docker must be installed &given permission to docker.sock  sudo chmod 666  /var/run/docker.sock

add jenkins in docker group: sudo usermod -aG docker jenkins 

Extra plugins required :
Maven plugin Integration
Docker plugin
Pipeline (install all pipeline )
tools :
maven :name :M3
Docker :name :myDocker

Remote server for Docker host & tomcat server
---------------------------------------------
It will be our second ec2 server.

Docker must be installed & given permission to docker.sock  sudo chmod 666  /var/run/docker.sock
Configure ssh between jenkins & remoteserver
--------------------------------------------
login jenkins server as jenkins user : sudo -iu jenkins

generate publickey : ssh-keygen -t rsa

normally it will in /var/lib/jenkins/.ssh folder
copy the public key

login into remote tomcat server --paste this key into .ssh/authorised_keys file

Check ssh is working from jenkins server to tomcat

Do the same steps in remote server & copy its public key to jenkins server(in jenkins server we need to create a new file authorised_keys & paste remote servers public key)

Integration between Jenkins & Docker
-------------------------------------

On jenkins server configure docker host by command : export DOCKER_HOST=“ssh://user@remotehost”

Test remote Docker host is working or not
-----------------------------------------

Create a freestyle job in jenins --select Execute shell from build step

type below contents: docker -H ssh://jenkins@172.31.28.25 run hello-world

you can use username ubuntu also.if we want to use jenkins user ,on remote server create a new user named jenkins & need to generate sshkey from that user and copy it to jenkins server authorised files

Run the job & check if it success.

Configure Docker hub for push image from jenkins
---------------------------------------------------

Configure credentails in jenkins with your docker hub username & password.Give the same ID as configured in Jenkinsfile 

Create pipeline job
--------------------
Note :modify your remote docker host ip & useranme before creating pipeline fron Jenkinsfile

Create pipeline job --select the pipeline from scm --give git repo : https://github.com/rjshk013/jenkins-tomcataws

Check the pipeline stages & finally check application is running on remote server as docker container

check from browser its working : http://public-ip:8003/LoginWebApp-1

reference : https://devops4solutions.com/integrate-jenkins-with-docker/
https://devops4solutions.com/ci-cd-using-jenkins-and-docker-2/




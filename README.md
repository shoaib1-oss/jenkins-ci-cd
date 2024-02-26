firstly we need to launch ec2 instance selecting ubuntu ami ,select ssecurity group ,give port number http port anywhere after that,select custom tcp port 8080 anywhere,after that save settings now launch instance
now goto instance connect copy ssh-client and open gitbash and paste the ssh here
#!/bin/bash
sudo apt update
sudo apt install openjdk-11-jdk -y
sudo apt install maven wget unzip -y

curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
  
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt-get update
sudo apt-get install jenkins -y
###
this cloud install jenkins
after that we get admin password from jenkins unlock the password use gitbash using commant (cat)
install selected plugins,after installing all plugins admin and user password and email
after that we need to go manage jenkins ,tools,install jdk now goto gitbash paste the command (ls /usr/lib/jvm)
copy[java_home (/usr/lib/jvm/java-11-openjdk-amd64)]paste this path [name(Orcalejdk11)]
apt install openjdk-8-jdk -y this will install oraclejdk8 
now goto maven it will install automatically select latest install
now build a freestyle job
at last build step select executable shell command write (id,whoami,pwd) and apply and build a job
now for artifacts from github we need to go github account and copy https url and paste at new fristly we need to build a new job for it
while building new job select source code management select git and paste the url here now at branch specifier select your branch while it is main or master branch
now at build step select invoke top levelmaven target Maven Version(maven) goals(clean install)
now build the artifact after building the artifact now goto workspace you will see all the file from github artifacts
there should be a pom.xml file for artifact to build a job


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
after that we get admin password from jenkins unlock the password use gitbash using command (cat)
install selected plugins,after installing all plugins admin and user password and email
after that we need to go manage jenkins ,tools,install jdk now goto gitbash paste the command (ls /usr/lib/jvm)
copy and paste at [java_home (/usr/lib/jvm/java-11-openjdk-amd64)]paste this path at [name (Orcalejdk11)]
apt install openjdk-8-jdk -y this will install oraclejdk8 
now goto maven it will install automatically select latest install
now build a freestyle job
at last build step select executable shell command write (id,whoami,pwd) and apply and build a job
now for artifacts from github we need to go github account and copy https url and paste at new fristly we need to build a new job for it
while building new job select source code management select git and paste the url here now at branch specifier select your branch while it is main or master branch
now at build step select invoke top levelmaven target Maven Version(maven) goals(clean install)
now build the artifact after building the artifact now goto workspace you will see all the file from github artifacts
there should be a pom.xml file for artifact to build a job
for artifact versioning we use at post  build action   step use **/*.war(for versioning use executable shell (mkdir -p versions cp target/vprofile-v2.war versions/vprofile-V$BUILD_ID.war)
for nexus use centos ami, t2 medium, create new key pair (ssh anywhere,custom tcp port 8081 anywhere here again custom tcp at port 8081 at source we need to give jenkins security group) now at user detail paste the nexus.setup script and launch instance
for sonarqube use ubuntu ami t2 medium  create new key pair (ssh anywhere,custam tcp port 80 anywhere,again custom tcp port 80 custom source same jenkins security group )discription (allow jenkins to connect sonarqube) at user data paste the setup of sonar 
nexus now copy public ip with port 8081 and paste on web browser after that at sign in it will ask user name and password use ssh in gitbash use cat command to get password
for sonar just copy public ip and paste it on web username and password will be (admin)
now goto manage jenkins u will see plugins use nexus artifact uploader,sonarqube sccanner,buildtime stamp, pipeline maven integration,pipeline utility steps,install all these)
now build a pipeline use create job pipeline  use pipelinescript
while making pipeline job we should see manage jenkins it should be install jdk8 with proper java_home after then only build the job
now goto manage jenkins you will see (sonarqube scanner name sonar4.7(install maven central same as name)
now at manage jenkins you will see system after goto sonarqube server put a checkmark at environmental vairiable)now at (sonarqube installaitions name sonar ,server url use(http://public ip)now for tokens goto sonarserver website at my account,security you will see generating tokens use name jenkins and copy token and paste in jenkins there will be secret text paste there )and apply now
for quality gate goto sonar website and there you will see quality gates create name (vprofile-QG) add condition (on overall code)quality gates fails when cognitive complexity (bugs)value(60) goto project and project setting quality gates select vprofile-qg
now project settings goto webhooks create webhooks name (jenkins-ci-webhook)url(http://jenkins private ip:8080/sonarqube-webhook)click on create
now goto jenkins security group and edit inbound add customtcp 8080 sonar-sg)save rule
at sonar website if test is failed beacuse of (60) we need to replace it with (100)
for nexus repository goto repository creat it maven 2 hosted name vprofile-repo now goto jenkins at jenkins goto credentials 
click on jenkins ,global crdentials,add credentials,username password will be admin)id and describtion will be nexuslogin)
now for paac_ sonar_nexus we need to do changes in ip adress use private ip address in the code after that goto manage jenkins at sonarqube installation change public to private ip,and in sonarway website webhooks goto projectsettings create webhook name jenkinsvprofile)(http://private ip of jenkins/8080/sonarqube-webhook) at settings symbol change jenkis public to private ip)now build the job

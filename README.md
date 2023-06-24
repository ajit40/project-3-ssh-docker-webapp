# project-3-ssh-docker-webapp
project-3-ssh-docker-webapp 

**Jenkins :** Install jenkins and connect with chrome terminal. <public-IP:8080> --> Steps : installations 

vi jenkins.sh 

sudo yum -y update
sudo amazon-linux-extras install java-openjdk11 -y
sudo tee /etc/yum.repos.d/jenkins.repo<<EOF
[jenkins]
name=Jenkins
baseurl=http://pkg.jenkins.io/redhat
gpgcheck=0
EOF
sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key
sudo yum install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
systemctl status jenkins
service jenkins restart
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Exec : sh jenkins.sh → Then connect into the chrome terminal.


**Docker :** Install docker  -->   Steps : installations

yum install docker -y
service docker start
service docker status
adduser dockeradmin
passwd dockeradmin
usermod -aG docker dockeradmin
visudo → add user access → dockeradmin ALL=(ALL) NOPASSWD: ALL
vi /etc/ssh/sshd_config → permitrootlogin : yes → passwordauthentication : yes 
service sshd restart
yum install git -y


**Step 1 :** su - dockeradmin → vi Dockerfile → paste these commands

![image](https://github.com/ajit40/project-3-ssh-docker-webapp/assets/120071904/0403172c-45fa-450c-abdd-8d05a5f78f22)

:wq

**Step 3 :** In jenkins

Add build step : send file or execute commands over SSH → transfer files → Source files : **/*.war → remove prefix : target → remove directory : / → exec command : add below given command

![image](https://github.com/ajit40/project-3-ssh-docker-webapp/assets/120071904/7a613164-b672-45a2-b9c0-b61020e12f17)

# exec command
docker stop qspider;
docker rm -f qspider;
docker image rm -f qspider;
cd /home/dockeradmin;
docker build -t qspider .

**Step 4 :** add post-build action : send build artifacts over SSH : Transfers : exec command :  add below given command

# exec command
docker run -it -d --name qspider -p 8090:8080 qspider

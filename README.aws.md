
https://hub.docker.com/r/dchevell/jira-core
dchevell/jira-core

http://cicd.services:8080
Admin User: admin/teamrocket
Standard User: workerbee/workerbee

http://cicd.services:8081
Admin User: admin/teamrocket
Standard User: workerbee/workerbee

Bitbucket is running on ports 8180 and 8181, with the same credentials, but are not enabled right now. They will be off until I get the jira testing worked out. 


https://github.com/ssgeejr/jira-syncing




https://community.atlassian.com/t5/Bitbucket-questions/Is-synchronization-possible-between-two-different-repo/qaq-p/89060

#jira  
http://cicd.services:8080  
http://cicd.services:8081  
  
#bitbucket  
http://cicd.services:8180  
http://cicd.services:8181  



docker run --detach --publish 7990:7990 cptactionhank/atlassian-bitbucket:latest

#### auto-mode  
```
#as devops
sudo mkdir -p /atlassian/jira/jalpha
sudo mkdir -p /atlassian/jira/jomega
sudo mkdir -p /atlassian/bitbucket/balpha
sudo mkdir -p /atlassian/bitbucket/bomega
sudo chown -R devops:devops /atlassian
sudo chmod -R 777 /atlassian

#fetch the build code
cd /jira
git clone https://github.com/ssgeejr/jira-syncing.git
cd jira-syncing

#switch to the version for AWS (ports are different and Bitbucket will be included)
git checkout -b aws

docker-compose up


```

```
adduser devops
cd /home/devops
mkdir .ssh

cat << EOF > /home/devops/.ssh/config
Host *
    StrictHostKeyChecking no
EOF

chmod 600 ./*
cd ..
chmod 700 .ssh
chown -R devops:devops .ssh

echo "%devops ALL=(ALL) NOPASSWD: LOG_INPUT: ALL"  > /etc/sudoers.d/scm-devops

hostnamectl set-hostname cicd.services

wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install -y epel-release-latest-7.noarch.rpm
rm -f epel-release-latest-7.noarch.rpm
yum install -y ftp://bo.mirror.garr.it/1/slc/centos/7.1.1503/extras/x86_64/Packages/container-selinux-2.21-1.el7.noarch.rpm

yum install -y wget curl git tmux python-pip python-dev build-essential lynx elinks tree lvm2
wget https://raw.githubusercontent.com/ssgeejr/config/master/.tmux.conf -O /etc/tmux.conf

curl -fsSL https://get.docker.com/ | sh

pip install -U pip
pip install --ignore-installed docker-compose

#yum search java-1.8.0-openjdk
#yum info java-1.8.0-openjdk
#yum install -y java-11-openjdk-devel
yum install -y java-1.8.0-openjdk-devel


#### Use this ONLY if you are adding a new partition for the docker library
#pvcreate /dev/nvme1n1 
#vgcreate dockervg /dev/nvme1n1
#lvcreate --name datalv1 -l 100%FREE dockervg
#mkfs.ext4 /dev/dockervg/datalv1
#rm -rf /var/lib/docker
#mkdir /var/lib/docker
#mount /dev/dockervg/datalv1 /var/lib/docker
#echo "/dev/dockervg/datalv1 /var/lib/docker ext4 defaults 0 0" >> /etc/fstab


systemctl start docker
systemctl enable docker

usermod -aG docker devops





```

### Add this to /etc/profile
```
JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.212.b04-0.el7_6.x86_64
M2_HOME=/opt/apache-maven-3.6.1
PATH=$M2_HOME/bin:$PATH
export JAVA_HOME M2_HOME PATH

```


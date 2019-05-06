# Jira-Sync

## Test project for synching two jira instances using the backbone-sync toolkit

## TODO  
1 license setup walkthrough  
2 install Backbone Sync  
3 test syncing 

### Before you get started make sure you have a Docker enabled VM (scroll down to Setup CentOS-7 VM)

## Notes for Jira Images and getting started

https://hub.docker.com/u/dchevell
https://hub.docker.com/r/zcalusic/atlassian-jira-core/
https://github.com/zcalusic/dockerfiles


#### manually start (sample)  
_coming soon, maybe_  

#### auto-mode  
```
#as devops
sudo mkdir -p /jira/local
sudo mkdir -p /jira/remote
sudo chown -R devops:devops /jira
sudo chmod -R 777 /jira

cd /jira
git clone https://github.com/ssgeejr/jira-syncing.git

cd jira-syncing

docker-compose up

```

#### you should see some output similar to this once it's online

```
remote    | 2019-05-02 19:57:50,561 JIRA-Bootstrap INFO      [c.a.jira.startup.LauncherContextListener] Memory Usage:
remote    |     ---------------------------------------------------------------------------------
remote    |       Heap memory     :  Used:   50 MiB.  Committed:  371 MiB.  Max: 1980 MiB
remote    |       Non-heap memory :  Used:   37 MiB.  Committed:   61 MiB.  Max: 1536 MiB
remote    |     ---------------------------------------------------------------------------------
remote    |       TOTAL           :  Used:   87 MiB.  Committed:  432 MiB.  Max: 3516 MiB
remote    |     ---------------------------------------------------------------------------------
local     | 2019-05-02 19:57:50,644 JIRA-Bootstrap INFO      [c.a.jira.startup.LauncherContextListener] Memory Usage:
local     |     ---------------------------------------------------------------------------------
local     |       Heap memory     :  Used:  104 MiB.  Committed:  371 MiB.  Max: 1980 MiB
local     |       Non-heap memory :  Used:   37 MiB.  Committed:   61 MiB.  Max: 1536 MiB
local     |     ---------------------------------------------------------------------------------
local     |       TOTAL           :  Used:  141 MiB.  Committed:  432 MiB.  Max: 3516 MiB
local     |     ---------------------------------------------------------------------------------
```


#### Now open two tabs in Chrome/Firefox and navigate to  
http://localhost:8080/  

http://localhost:8880/  

You should see the notes to add a (test) licences which takes about 2 minutes.  You can install the Backbone software and start testing    

### Apply a test license to the two instances 








#### manually start the instance (ignore this unless you know exactly what you are doing)  
```
sudo docker run -d -p 8080:8080 -v your-jira-home:/var/atlassian/application-data/jira --name jira zcalusic/atlassian-jira-core
```

#### using docker-compose (use this)  
```
docker-compose up 
```

## Setup CentOS-7 VM
### Build or Download an Images
You can do this a number of ways, dealers choice  

**Install an OS manually**  
[CentOS](https://www.centos.org/download/ "CentOS Distributions")  
[Debian](https://www.debian.org/distrib/ "Debian Distributions")  

**Download a pre-built image for VirtualBox**  
[OSBoxes Virtualbox Images](https://www.osboxes.org/virtualbox-images/ "VirtualBox Images")  

### Configure your image for Docker testing
```
#as root
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

wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
yum install -y epel-release-latest-7.noarch.rpm
rm -f epel-release-latest-7.noarch.rpm
yum install -y ftp://bo.mirror.garr.it/1/slc/centos/7.1.1503/extras/x86_64/Packages/container-selinux-2.21-1.el7.noarch.rpm

yum install -y wget curl git tmux python-pip python-dev build-essential lynx elinks tree
wget https://raw.githubusercontent.com/ssgeejr/config/master/.tmux.conf -O /etc/tmux.conf

curl -fsSL https://get.docker.com/ | sh

pip install -U pip
pip install --ignore-installed docker-compose

systemctl start docker
systemctl enable docker

usermod -aG docker devops

#log out, log back in for group settings to take effect
```





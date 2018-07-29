# roshi-challenge
This repo is a set of Ansible playbooks to automate the deployment of proxy, Roshi and Redis on Ubuntu 16.04. 

## Getting Started

These instructions includes the setup the environment of ansible nodes and the commands of running playbooks.

### Prerequisites

**Control node** need to have Ansible installed ( newer than version 2.3 ).  Need to configure /etc/hosts for other hosts. Need to have golange installed. 

To install ansible:
```
$ apt-get update
$ apt-get install software-properties-common
$ apt-add-repository ppa:ansible/ansible-2.6
$ apt-get update
$ apt-get install ansible
```

To install  go-lang:
```
$ apt install golang-go
$ vim ~/.bashrc
export GOPATH=$HOME/go
     	export PATH=$PATH:$GOROOT/bin:$GOPATH/bin
$ source ~/.bashrc
```
To configure hosts (need to adjust ip address)
```
$ vim /etc/hosts
127.0.0.1 localhost
172.31.8.5 proxy01
172.31.8.5 roshi01
172.31.6.165 redis01
127.0.0.1 control01
127.0.0.1 roshi
```
To setup ssh connection to other server:
On Control node: 
$ ssh-keygen
$ cat ~/.ssh/id_rsa.pub

Copy the out put phrase to Roshi and Redis node /root/.ssh/known_hosts. 
Change /etc/ssh/sshd_config save and reboot
```
PermitRootLogin yes
```

### Installing
```
git clone https://github.com/Toucher-F/roshi-challenge.git
```
Need to adjust the ip address of redis server in playbook
```
vim roshi-challenge/roles/roshi-deploy/defaults/main.yml
  redis_address: 172.31.6.165
```

## Running the testing
To run the complete playbook:
```
ansible-playbook site.yml
```
To only install and run redis to redis server:
```
ansible-playbook redis.yml
```
To only install and run nginx to proxy server:
```
ansible-playbook proxy.yml
```
To only download and build roshi on control server:
```
ansible-playbook roshi-build.yml
```
To only deploy roshi to roshi server:
```
ansible-playbook roshi-deploy.yml
```
To check the service status of servers:
```
ansible-playbook status-check.yml
```
To restart redis, proxy and roshi on servers:
```
ansible-playbook stack-restart.yml
```

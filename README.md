# Test
setting up the infrastructure to installing software



===> Create two EC2 Instances in AWS Cloud using,
------------------------------------------------------
          Additional Information
Instance Type of both instance is t2.micro
Operating System for both instances Ubuntu Server 16.04 LTS
Hostname of Instance 1 : MSR-test-Instance-1
Hostname of Instance 2 : MSR-test-Instance-2

Launch Instance
Step 1: *Choose an Amazon Machine Image (AMI)
         select Ubuntu Server 16.04 LTS
        *Choose an Instance Type:
         t2.micro
        *Configure Instance Details
         Number of instances=2
        *Configure Security Group
         Assign a security group:Select Required port numbers
         create a public key pair and convert that public key into private key with help of putty gen.             
         Login in putty with private key

Login:ubuntu
Hostname of Instance:
-----------------------
command:sudo hostname MSR-test-Instance
or add hostname in sudo vi /etc/hosts

===> configuration management tool :Ansible
--------------------------------------
step 1:install ansible --sudo apt-get install ansible -y
By default it will download its dependecies phyton.

step2: Run ssh-keygen command and simply enter 3 times
command:cd .ssh
        copy  id_rsa.pub key and paste in authorized keys
        Repeat same step --copy  id_rsa.pub key and paste in authorized keys for instance 2

step3:Hosts file of ansible
command:sudo vi /etc/ansible/hosts
        enter public id of both instances.
        Varify hosts file (ansible all --list-hosts)
        Now run ansible command to ping(ansible all -m command -a date)
        if found any error please check publickey authentication is yes
        permitrootlogin in sudo vi etc/ssh/sshd_config
        restart servie:service sshd restart
       
 ===>Installing packages by using a configuration management tool Ansible:
 create a yml file
 command:sudo vi test.yml
 
 script:
 -----------------------------------------------------------
---
- hosts: all
  become: true
  tasks:

  - name: Install git
    apt: pkg=git 

  - name: Add Docker GPG key
    apt_key: url=https://download.docker.com/linux/ubuntu/gpg

  - name: Add Docker APT repository
    apt_repository:
      repo: deb [arch=amd64] https:https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
  - name: Install list of packages
    apt:
      name: ['apt-transport-https','ca-certificates','curl','software-properties-common','docker-ce']
      state: present
      update_cache: yes
      
  - name: nodejs 8.12
    get_url:
      url=https://nodejs.org/dist/v8.12.0/node-v8.12.0.tar.gz
      dest=/opt
  - name: Extract node tar.xz
    unarchive:
      src: /opt/node-v8.12.0-linux-x64.tar.xz
      dest: /opt/riyaz
      remote_src: yes
  - name: Rename
    command: mv /opt/riyaz/node-v8.12.0 /opt/node-v8
  - name: create 'nodejs-env.sh' 
    file: path=/etc/profile.d/nodejs-env.sh state=touch owner=root group=sys mode=0555
  - name: content 
    copy:
     dest: /etc/profile.d/nodejs-env.sh/   
     content:
      export NODEJS_HOME=/etc/node/node-v8.12.0/bin
      export PATH=/etc/node/node-v8.12.0/bin:/etc/node/node-v8.12.0

  
----------------------------------------------------------
  by this method it installed a stable version
  //- name: Install docker-compose
    apt: pkg=docker-compose

  - name: Install Nodejs
    apt: pkg=nodejs//
---------------------------------------------------------------
  - name: Install Docker Compose.
    get_url:
        url: https://github.com/docker/compose/releases/download/1.23.2/docker-compose-Linux-x86_64
        dest: /usr/local/bin/docker-compose
        mode: 4777




 
 
 
       
         

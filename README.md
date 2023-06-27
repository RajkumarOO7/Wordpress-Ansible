                   Wordpress-Ansible-Automation

WordPress+Apache+PHP+MariaDB Deployment with required dependencies on CentOs7


1. Created two instances, Controller-node and Target-server
  
2. Generated public and private key in Controller node
    $ ssh-keygen
    $ cd .ssh/
    $ cat id_rsa.pub
      ctrl+c
3. Copied public key from Controller-node to Target-server
    $ cd .ssh/
    $ vi authorized_keys
      ctrl+v

4. SSH to Target-server from Controller-node
    $ ssh root@64.225.105.232
    $ exit
   
5. Installing Ansible and its dependencies
    $ yum update -y
    $ yum install epel-release
    $ yum install ansible -y
    $ ansible --version
      
6. Updated the hosts file under /etc/ansible directory to include the names or URL's of the servers i want to deploy.
    $ cd /etc/ansible
    $ ll
    $ vi hosts
       [wordpress]
       ip address of the Target-server

 7. Trying to ping with Target-server
    $ ansible wordpress -m ping
    
 8. Installing Git without Playbook.yml
    $ vi gitplaybook.yml

 9. Executing Playbook.yml
     $ ansible-playbook gitplaybook.yml

 10. Cloning on Ansible
     ansible localhost -m git -a "repo=https://github.com/RajkumarOO7/Wordpress-Ansible.git dest=/etc/ansible/gitwordpress" -b

 11. Running Playbook.yml
     $ cd /etc/ansible/gitwordpress
       

  Then run the playbook with the the command 
      $ ansible-playbook wordpressplaybook.yml





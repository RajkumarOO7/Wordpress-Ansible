Wordpress-Ansible-Automation

WordPress+Apache+PHP+MariaDB Deployment with required dependencies on CentOs7

Playbooks deploy a simple all-in-one WordPress configuration ready to be configured after the installation.

Update the hosts file under /etc/ansible directory to include the names or URL's of the servers you want to deploy. There is one example hosts file in the repository.

Uploaded private key of Controller node to authorized key in Target server

Then run the playbook with the below command:

ansible-playbook wordpressplaybook.yml

Happy blogging.

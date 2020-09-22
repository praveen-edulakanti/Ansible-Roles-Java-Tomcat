Setting hostname in "Target Server": 

   sudo hostnamectl set-hostname nodeserver-1

Steps: Password less Authentication in "Ansible Controller Server" and "Target Server"(nodeserver-1)

1) Create same user(ansadmin) across all servers and provide password  for all users.

	sudo useradd -m ansadmin
	
	sudo passwd ansadmin
	
2) Add username in visudo

        visudo  (add  "ansadmin ALL=(ALL) NOPASSWD:ALL")
	
3)  Change to bash

    sudo vi /etc/passwd == change /bin/sh to /bin/bash

4)  Make sure that PasswordAuthentication yes in all servers under /etc/ssh/sshd_config file

	sudo vi /etc/ssh/sshd_config
	
	sudo systemctl restart sshd
	
5)  Generate ssh-keys using ssh-keygen command from ansadmin.

From "Ansible Controller Server":

1) Copy ssh public key using ssh-copy-id  <hostname> from  /home/ansadmin/.ssh/ location.
	
	ssh-copy-id nodeserver-1_IPAddress
 
2) Now login to remote server without providing password with the following command:
	
	ssh username@hostname
	
        or 
	
	ssh nodeserver-1_IP from ansadmin user
 
3) Now we can test connection from Ansible Engine to Mange Node using: 
  
     ansible all -m ping 

  For Installation of Java and Tomcat run below playbook and change versions in vars folder.
  
     ansible-playbook install_config.yaml

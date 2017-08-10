# -*- mode: ruby -*-
# vi: set ft=ruby :
#
#Here is the project description: 
#The goal of this project is to use Vagrant to build a VM that is configured automatically. Your automation should fully patch the VM, #install git; mysql; and apache, and then automatically test if each application is installed. After this check, your automation should #clone a git repository of your choice to the Vagrant VM in the /opt/code directory. The owner and group of the cloned files should be #pythian:pythian.

#This file contains the code required to successfully finish the project. 

#Please refer to the solution_guide for the detailed description of the methodology used to tackle the project. 



Vagrant.configure("2") do |config|		
	#This configuration uses "ubuntu/xenial64" box. 
	config.vm.box = "ubuntu/xenial64"		
 	# Your implementation goes here	
	
	#config.vm.provision "shell" do |sl|
		#sl.path = “bootstrap"

	#end	

	config.vm.provision "shell", inline: <<-SHELL

		echo “installing Apache2”
		apt-get update
		apt-get install -y apache2
		if ! [ -L /var/www ]; then
		  rm -rf /var/www
		  ln -fs /vagrant /var/www
		fi

		echo “installing Git”
		apt-get install git -y > /dev/null

		echo “installing MySQL”
		apt-get install debconf-utils -y > /dev/null
		debconf-set-selections <<< "mysql-server mysql-server/root_password password Pass12"
		debconf-set-selections <<< "mysql-server mysql-server/root_password_again password Pass12"
		apt-get update
		apt-get -y install mysql-server > /dev/null

		if [ $(dpkg-query -W -f='${Status}' git 2>/dev/null | grep -c "ok installed") -eq 1 ];
		then 
			echo “git… Installed”
		fi

		if [ $(dpkg-query -W -f='${Status}' mysql 2>/dev/null | grep -c "ok installed") -eq 0 ];
		then 
			echo “mysql… Not Installed!”
		fi

		if [ $(dpkg-query -W -f='${Status}' apache2 2>/dev/null | grep -c "ok installed") -eq 1 ];
		then 
			echo “apache… Installed”
		fi

		sudo adduser pythian --gecos "First Last,RoomNumber,WorkPhone,HomePhone" --disabled-password

		mkdir -p opt/code
		git clone https://github.com/pythian/DevOps_Cloud_Automation_Engineer_Project.git opt/code

		chmod 750 -R opt/code
		sudo chown -R pythian:pythian opt/code
	SHELL
 		
end

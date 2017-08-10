# -*- mode: ruby -*-
# vi: set ft=ruby :

###
#Here is the project description: 
#The goal of this project is to use Vagrant to build a VM that is configured automatically. Your automation should fully patch the VM, #install git; mysql; and apache, and then automatically test if each application is installed. After this check, your automation should #clone a git repository of your choice to the Vagrant VM in the /opt/code directory. The owner and group of the cloned files should be #pythian:pythian.

#This file contains the code required to successfully finish the project. 

#Please refer to the solution_guide.md for the detailed description of the methodology used to tackle the project. 
###


Vagrant.configure("2") do |config|		
	#This configuration uses "ubuntu/xenial64" box. 
	config.vm.box = "ubuntu/xenial64"		



	#This is the first way of doing the project. An alternate way is presented after the shell provisioning. 
	#We use shell script to automate the required tasks outlined in the solution_guide. 
	config.vm.provision "shell", inline: <<-SHELL
		
		#Apache2 installation 
		echo “installing Apache2”
		apt-get update
		apt-get install -y apache2
		if ! [ -L /var/www ]; then
		  rm -rf /var/www
		  ln -fs /vagrant /var/www
		fi

		#Git installation
		echo “installing Git”
		apt-get install git -y > /dev/null

		#MySQL installation
		echo “installing MySQL”
		#MySQL installation requires setting up a password. 
		#To automate this, I used debconf-utils as shown in the following three lines of code.
		apt-get install debconf-utils -y > /dev/null
		debconf-set-selections <<< "mysql-server mysql-server/root_password password Pass12"
		debconf-set-selections <<< "mysql-server mysql-server/root_password_again password Pass12"
		
		apt-get update
		apt-get -y install mysql-server > /dev/null

		#Installation of the required software is done! 		

		#The following three if-statements are used to test whether git, MySQL, and Apache has been installed.
		#It first check the status of a package, if the status contained “ok installed” returns 1, otherwise returns 0.
		#For git, since the example shows that it should be installed, we check if the returned value is 1, then print 
		#a custom message.
		if [ $(dpkg-query -W -f='${Status}' git 2>/dev/null | grep -c "ok installed") -eq 1 ];
		then 
			echo “git… Installed”
		fi
		
		#Here we check whether MySQL has been installed. The custom message is also created. 
		if [ $(dpkg-query -W -f='${Status}' mysql 2>/dev/null | grep -c "ok installed") -eq 0 ];
		then 
			echo “mysql… Not Installed!”
		fi

		#Here we check whether Apache has been installed. The custom message is also created. 
		if [ $(dpkg-query -W -f='${Status}' apache2 2>/dev/null | grep -c "ok installed") -eq 1 ];
		then 
			echo “apache… Installed”
		fi
		
		#The following line of code is to automatically add the use “pythian” to the system
		#Creating a new user is an interactive process, which has to be handled.
		sudo adduser pythian --gecos "First Last,RoomNumber,WorkPhone,HomePhone" --disabled-password

		#We skipped setting up a password for the user. It can also be included using the following code:  
		#echo "myuser:password" | sudo chpasswd

		#Making the required directory and clone a git repository in that directory
		mkdir -p opt/code
		git clone https://github.com/pythian/DevOps_Cloud_Automation_Engineer_Project.git opt/code

		#The ownership and permission is change for the code directory and all of the files within that
		#to “drwxr-x--- 3 pythian pythian 4096 Aug 10 15:30 code” (This is the output that I got during the testing procedure).
		chmod 750 -R opt/code
		sudo chown -R pythian:pythian opt/code
	SHELL
	#This is the end of the project.
	
	#This part is an alternative way of doing the project.
	#We first provision shell and use it to call a separate shell-script file on the same path as vagrantfile.
	#The “bootstrap” file contains all the required tasks. 
	#Finally, we end the shell.
	
	#config.vm.provision "shell" do |sl|
	#	sl.path = "bootstrap"

	#end	
 		
end

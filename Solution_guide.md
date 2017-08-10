# DevOps Cloud Automation Engineer Project

## Project description:

The goal of this project is to use Vagrant to build a VM that is configured automatically.  Your automation should fully patch the VM, install git; mysql; and apache, and then automatically test if each application is installed.  After this check, your automation should clone a git repository of your choice to the Vagrant VM in the /opt/code directory.  The owner and group of the cloned files should be pythian:pythian.

## Other Details

The /opt/code ownership and permissions should match the state below.

```
drwxr-x--- 0 pythian pythian 512 Jul 14 08:03 code
```

When testing if the applications are installed, your automation should produce output consistent with the following example

```
git... Installed
mysql... Not Installed!
apache... Installed
```
## Solution Methodology

To solve this problem, I broke the project into smaller pieces as below. Each line with a number describes a goal or a task. 

1. Vagrant is going to be used to build a VM.
  * Install virtualbox 5.1.26 (My prefered virtual machine).
  * Install Vagrant 1.9.7.
2. Configure Vagrantfile to install git, mysql, and apache.
3. Configure Vagrantfile to test wether the installation of those packages has been consistent with the given example.
4. Clone a git repository in the /opt/code directory
  * First, make /opt/code directory, then clone a git repository in it. 
5. Add a user and group named as *pythian*.
6. Change the ownership and permission of the /opt/code directory and all of the files within to the *pythian* user. 
  * The ownership and permission for the user and group should be consistent with the given example.


  


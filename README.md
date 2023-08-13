# vagrant-projects
All projects relating to Linux and vagrant

# After you have configured your Virtual Machine.

Create a folder to keep your course work in.  First, start a command line session on your local machine. 
For Windows users, start the Command Prompt.  (Click the Start button. In the Search box, type "git bash and then, double-click on it.) 
For Mac users, start the Terminal application which is located in the /Applications/Utilities 
folder. 
mkdir linuxclass1 
When the Command Prompt (Windows) or the Terminal (Mac) is launched you will be placed in your home directory.  For example, if I’m logged into a Windows system as "jason" my home directory could be "C:\Users\jason".  (Note: this might vary depending on the version of Windows you are using.)  If I’m logged into a Mac system as "jason" my home directory will be "/Users/jason". 
Change into the Working Folder 
Now let's move into the folder we just created. 
cd linuxclass1 
Add a Box to Vagrant 
A "box" in Vagrant speak is an operating system image.  The "vagrant box add" command will download and store that box on your local system.  You only need to download a box once as this image will be cloned when you create a new virtual machine with Vagrant using the box’s name. 
I created a box specifically for this class and uploaded it to the public Vagrant box catalog.  Run the following command on your local machine to download it. 
vagrant box add jasonc/centos8 
The format of the command when downloading a public box is "vagrant box add USER/BOX". There are several public boxes available to download.  You can search for boxes here, but be sure to use the "jasonc/centos8" box for this class. https://app.vagrantup.com/boxes/search 
Create a Vagrant Project Folder 
Vagrant uses the concept of projects.  A Vagrant project must consist of a folder and a Vagrant configuration file, called a Vagrantfile.  Start out by creating a "myfirstvm" folder. 
mkdir myfirstvm 
Create Your First Vagrant Project 
To create the Vagrant configuration file (Vagrantfile), run the "vagrant init <BOX_NAME>" command.  Be sure to be in Vagrant project directory you just created.  Also, use the "jasonc/centos8" box you downloaded earlier. 
cd myfirstvm vagrant init jasonc/centos8 
Create Your First Virtual Machine 
The first time you run the "vagrant up" command Vagrant will import (clone) the vagrant box into VirtualBox and start it.  If Vagrant detects that the virtual machine already exists in VirtualBox it will simply start it.  By default, when the virtual machine is started, it is started in headless mode meaning there is no UI for the machine visible on your local host machine. 
Let’s bring up your first virtual machine running Linux with Vagrant. 
vagrant up 
VirtualBox Guest Additions Version 
If you see a warning about guest additions, ignore it. The VirtualBox guest additions allow us to share the vagrant project directory on our local computer with the VM at /vagrant. Even if the guest additions version on the VM is lower than the version of VirtualBox you are running on your computer, it will work. 
Here's the warning message you might see: 
The guest additions on this VM do not match the installed version of VirtualBox! In most cases this is fine, but in rare cases it can prevent things such as shared folders from working properly. If you see shared folder errors, please make sure the guest additions within the virtual machine match the version of VirtualBox you have installed on your host and reload your VM. 
Guest Additions Version: 6.1.12 
VirtualBox Version: 6.2 
Troubleshooting 
On some systems, you may see the following message, though it is rare. 
Timed out while waiting for the machine to boot. This means that Vagrant was unable to communicate with the guest machine within the configured ("config.vm.boot_timeout" value) time period. If you look above, you should be able to see the error(s) that Vagrant had when attempting to connect to the machine. These errors are usually good hints as to what may be wrong. 
If you're using a custom box, make sure that networking is properly working and you're able to connect to the machine. It is a common problem that networking isn't setup properly in these boxes. Verify that authentication configurations are also setup properly, as well. 
If the box appears to be booting properly, you may want to increase the timeout ("config.vm.boot_timeout") value. 
If you do see that message, it most likely means the virtual machine started with its networking interface disabled.  To force the network interface to be enabled, we'll need to update the Vagrantfile. The Vagrantfile controls the behavior and settings of the virtual machine.  Open it with your favorite text editor.  You may need to use the File Explorer (Windows) or the Finder (Mac) to navigate to the folder and then open it with your desired editor. 
By the way, Atom is a nice text editor that works on Mac, Windows, and even Linux.  You can download it for free at Atom.io. 
Add the following line somewhere after "Vagrant.configure(2) do |config|" and before "end".  A good place could be right after the 'config.vm.box = "jasonc/centos8"' line: 
config.vm.provider "virtualbox" do |vb| vb.customize 
['modifyvm', :id, '--cableconnected1', 'on'] end 
Reboot the virtual machine with Vagrant. 
vagrant reload 
Confirm That It's Running 
Start the VirtualBox application.  On Windows, double-click on the "Oracle VM VirtualBox" icon on your desktop.  For Mac users, start the /Applications/VirtualBox application. 
Confirm that you see a virtual machine running.  It will start with the name of your Vagrant project folder. 
You can also use the "vagrant status" command to check the status of the virtual machine.  Confirm that it shows the virtual machine is in a running state. 
vagrant status 
Connect to the Virtual Machine 
SSH, secure shell, is the network protocol used to connect to Linux systems.  Vagrant provides a nice shortcut to ssh into the virtual machine. 
vagrant ssh 
Now you are connected to the Linux virtual machine as the vagrant user.  This default vagrant account is used to connect to the Linux system.  For your convenience, the Vagrant application takes care of the details that allow you to connect to the box over SSH without a password.  For reference, the password for the vagrant account is "vagrant".  The password for the root account is also "vagrant".  The vagrant user has full sudo (administrative) privileges that allow you to further configure the system.  You will learn more about accounts, privileges, and sudo later in this course. 
After running "vagrant ssh" you should be presented with a prompt that looks similar to this: 
  
You'll be working a lot at the Linux command line in this course.  For now, let’s log out of the Linux system by running the "exit" command. 
$ exit 
Note: when you see a dollar sign it’s not part of the command.  It’s a placeholder for a Linux prompt. Even if your prompt is "[vagrant@localhost ~]$", it will be abbreviated to just "$" in the examples. 
Stop the Virtual Machine 
The "vagrant halt" command shuts down the virtual machine.  When you run this command you will not lose any work you’ve performed on the virtual machine.  The virtual machine will still exist in VirtualBox, it will simply be stopped. 
vagrant halt 
Open the VirtualBox application and verify that the virtual machine is still defined, yet stopped. 
Start the Virtual Machine Again 
Remember, to start the virtual machine, run "vagrant up".  This time when you run the command it will not need to import the box image into VirtualBox as the virtual machine already exists. 
Switch back to the command line on your local machine and run the following command. 
vagrant up 
Open the VirtualBox application and verify that the virtual machine is now running. 
Destroy the Virtual Machine 
If you are done with the virtual machine, or you want to start over with a fresh copy of the virtual machine, run "vagrant destroy". 
vagrant destroy 
It will prompt you to confirm that you want to delete the virtual machine.  Answer "y".  If the virtual machine is running, it will stop it and then delete it.  If it’s already stopped, it will simply delete it. 
Create Another Vagrant Project and Virtual Machine 
Let’s create another Vagrant Project and virtual machine.  First, let’s return to our "linuxclass" directory.  The "cd .." command changes to the parent directory which is represented by "..". 
cd .. 
Next, let’s create the Vagrant project folder and change into that folder. 
mkdir testbox01 cd testbox01 
Initialize the Vagrant project.  This step creates the Vagrantfile. 
vagrant init jasonc/centos8 
Start the virtual machine. 
vagrant up 
Connect to the virtual machine to confirm that it’s working and then exit it. 
vagrant ssh 
$ exit 
When you connect, you will see a prompt similar to this one: 
  
Change the Virtual Machine's Name 
The Vagrantfile controls the behavior and settings of the virtual machine.  Open it with your favorite text editor.  You may need to use the File Explorer (Windows) or the Finder (Mac) to navigate to the folder and then open it with your desired editor. 
By the way, Atom is a nice text editor that works on Mac, Windows, and even Linux.  You can download it for free at Atom.io. 
Add the following line somewhere after "Vagrant.configure(2) do |config|" and before "end".  A good place could be right after the 'config.vm.box = "jasonc/centos8"' line: 
config.vm.hostname = "testbox01" 
Be sure to save your changes. 
At this point, you could run "vagrant halt" followed by "vagrant up" to activate this change. 
However, Vagrant provides a shortcut: "vagrant reload" which restarts the virtual machine, loads the new Vagrantfile configuration, and starts the virtual machine again.  Give it a go: 
vagrant reload 
Connect to the virtual machine to confirm that it's hostname has changed. 
vagrant ssh 
You should see a prompt similar to this one containing the hostname that you configured: 
  
Disconnect from the virtual machine: 
$ exit 
Assign the Virtual Machine an IP Address 
During this course, you are going to create an entire (private) network of virtual machines that will be able to communicate with each other as well as your local workstation.  Let's give this virtual machine the IP address of "10.23.45.10".  To do that, insert the following line of configuration into the Vagrantfile.  Remember, it needs to be inserted somewhere after "Vagrant.configure(2) do |config|" and before "end". 
config.vm.network "private_network", ip: "10.23.45.10" 
Save your changes. 
Reload the virtual machine and let Vagrant assign the IP address to it. 
vagrant reload 
Test the connection by pinging the virtual machine. 
Window users, run the following command: 
ping 10.23.45.10 
Mac users, run the following command: 
ping -c 3 10.23.45.10 
The ping command is one simple way to test network connectivity.  If you see replies, then you can safely assume the IP address is reachable and the host is up.  If you see "timeout" messages, then the system is not answering your ping requests.  In the "real world" this doesn't necessarily mean the system is "down."  It means it is not answering your ping requests which could be for a variety of reasons.  However, for our purposes here, if you get "timeout" messages, then you can assume this system is down or something is wrong.  The first thing to try is to simply reboot the VM by running: 
vagrant reload 
If the ping command fails again, double-check the contents of the Vagrantfile paying special attention to the config.vm.network line. Make any necessary changes, restart the virtual machine and try again. 
The final step is to reboot the host operating system, I.E. your physical computer.  This troubleshooting step primarily applies to Windows users. 
Stop the Virtual Machine 
In upcoming projects you'll be working more with Vagrant, virtual machines, IP addresses and more. Feel free to explore the Linux system if you'd like.  (Connect by running "vagrant ssh" within the project folder.)  When you're ready to stop or take a break, halt the virtual machine.  Remember, you can always pick up where you left off as long as you don't destroy the virtual machine. 
vagrant halt 
Final Vagrantfile for testbox01 
Here are the contents of the linuxclass/testbox01/Vagrantfile file with all the comments removed. 
Vagrant.configure(2) do |config| config.vm.box = "jasonc/centos8" config.vm.hostname = "testbox01" 
config.vm.network "private_network", ip: "10.23.45.10" end 

mkdir linuxclass1 and cd linuxclass1

 <img width="393" alt="image" src="https://github.com/Hardey1774/vagrant-projects/assets/111874994/3bf765b6-7a94-4e33-a9fe-22c9896d8073">



<img width="646" alt="image" src="https://github.com/Hardey1774/vagrant-projects/assets/111874994/69f20a8f-b43b-4222-8e6d-536bd22d43be">




<img width="299" alt="image" src="https://github.com/Hardey1774/vagrant-projects/assets/111874994/7dc3f514-8018-4727-b626-6a1c8892b028">



<img width="372" alt="image" src="https://github.com/Hardey1774/vagrant-projects/assets/111874994/8b0bb8e5-d3f9-4371-a711-664275f87c79">



<img width="476" alt="image" src="https://github.com/Hardey1774/vagrant-projects/assets/111874994/d7acbbf2-6238-47d4-8e9f-9d61a86443da">




exit the server and vi into vagrantfile, change the hostname with config.vm.hostname = "myfirstvm" save and quit, 

<img width="476" alt="image" src="https://github.com/Hardey1774/vagrant-projects/assets/111874994/f4af02aa-e84c-4c29-b1d0-dd0a70954a5a">




reload server with "vagrant reload"

<img width="535" alt="image" src="https://github.com/Hardey1774/vagrant-projects/assets/111874994/2e912926-9b16-43b8-b4ca-57a44ebc6b4a">




ssh into the server with "vagrant ssh" and do "hostname" to see if the name has changed

<img width="535" alt="image" src="https://github.com/Hardey1774/vagrant-projects/assets/111874994/56366f0a-f2a5-40de-a26c-06c2ad9ae072">




<img width="310" alt="image" src="https://github.com/Hardey1774/vagrant-projects/assets/111874994/ad2ed05c-6183-45f0-8287-f42745cc3440">





exit the server and assign an ipaddress (10.23.45.10) with config.vm.network "private_network", ip: "10.23.45.10", save and quit then reload

<img width="482" alt="image" src="https://github.com/Hardey1774/vagrant-projects/assets/111874994/3b05dc24-9e29-4653-b6c5-f955b8c018a2">




ping the server using ping 10.23.45.10 on another terminal

<img width="436" alt="image" src="https://github.com/Hardey1774/vagrant-projects/assets/111874994/31091027-0b33-4403-b13b-3a3e4ea67cd0">




You can pause the server using "vagrant halt" , destroy the server using "vagrant destroy"

<img width="426" alt="image" src="https://github.com/Hardey1774/vagrant-projects/assets/111874994/0ff65b5e-8f31-454a-93d2-1592f50f3947">



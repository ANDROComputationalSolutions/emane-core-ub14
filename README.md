<b>EMANE v0.9.3 and CORE snapshot Ubuntu 14.04 Vagrant Box</b>

<b>U.S. NAVAL RESEARCH LABORATORY - PRODUCTS</b>
https://www.nrl.navy.mil/itd/ncs/products

<b>EMANE</b>
https://www.nrl.navy.mil/itd/ncs/products/emane

<b>CORE</b>
https://www.nrl.navy.mil/itd/ncs/products/core


<b>OPTIONAL PRESTEP:</b>
Install the vBox Guest additions manager by entering the following command:

vagrant plugin install vagrant-vbguest


<b>SETUP NOTES:</b>
There are two ways to use this box, through ssh or using a desktop installed.

As default, the desktop is used. If you do not want to use the desktop, then go to the Vagrantfile,
and uncomment the correct config.vm.box_url address.

If you previously launched the vagrant with this vagrant file, and want to switch the base box, then
you must delete your current vagrant and remove the cached vagrant box. you can do this with the following commands:

vagrant destroy
vagrant box remove emane-basebox

<b>SETUP INSTRUCTIONS:</b>
The first step will install and configure EMANE, CORE, and all deps. This step will
take several minutes. The next step shuts down the machine. The third step 
restarts the machine with the correct configuration. If vagrant-vbguest is installed,
then it will install the correct guest additions package as well.

vagrant up  
vagrant halt  
vagrant up  

<b>ACCESSING VAGRANT VM:</b>
Via SSH: vagrant ssh  
Via Desktop: In VirtualBox, click "Show" once VM is Running
Username: vagrant
PW: vagrant

<b>ADDING EMANE TUTORIAL HOSTNAMES:</b>
Add the following entries into /etc/hosts:  

10.99.0.1 node-1  
10.99.0.2 node-2  
10.99.0.3 node-3  
10.99.0.4 node-4  
10.99.0.5 node-5  
10.99.0.6 node-6  
10.99.0.7 node-7  
10.99.0.8 node-8  
10.99.0.9 node-9  
10.99.0.10 node-10  
10.99.0.100 node-server  
10.100.0.1 radio-1  
10.100.0.2 radio-2  
10.100.0.3 radio-3  
10.100.0.4 radio-4  
10.100.0.5 radio-5  
10.100.0.6 radio-6  
10.100.0.7 radio-7  
10.100.0.8 radio-8  
10.100.0.9 radio-9  
10.100.0.10 radio-10  


<b>SETTING SSH KEY FOR EMANE TUTORIAL</b>
The EMANE Tutorial uses LXC containers to create virtual nodes. To use a public 
key to log into each node, create the key and copy with yourself.  

ssh-keygen  
ssh-copy-id vagrant@localhost  


<b>TESTING EMANE:</b>
Once inside the VM, there will be an emane-tutorial folder in the home directory.
In this directory, several demos from 0-8 from Adjacent Link. More information on
what each demo shows can be seen at: https://github.com/adjacentlink/emane-tutorial

For now, we will just use the first demo to check if everything has been
properly installed with the steps below:

cd ~/emane-tutorial/0  
make  
rm -rf NO-*  
sudo ./demo-start  
../scripts/olsrlinkview.py  

From there, an Xforwarded window should pop up with nodes 1 and 2 active. GPS takes
30 seconds to start showing pseudo locations on the right. In the left window, the nodes
are all placed in the top left, these can be dragged around on the screen to see their 
current connections. If all these work, then all of emane should be working and you can
continue with the tutorial. Once done, shut down the demo by stopping the gui and entering 
the following code below. If nodes 1 or 2 are not active(shown in red), then try 
shutting off the demo and starting it again. Sometimes the LXC containers do not 
launch properly.

sudo ./demo-stop  


<b>TESTING CORE:</b>
Core has two parts, the daemon which runs in the background as a service and the gui.
Enter the following to start the daemon and open the CORE GUI.  

sudo service core-daemon start 
OR 
sudo core-daemon

core-gui  

The Core gui should launch and say CORE(*PORT* on vagrant.local) at the top. If the daemon
is not running than there will be no port.


<b>CORE GUI EXAMPLES</b>
When Core is installed, example scripts will be placed in /home/vagrant/.core/configs . These 
files can be directly opened with the GUI.


<b>CORE PYTHON EXAMPLES</b>
CORE has built in python API bindings. The python script can be run standalone or through the 
GUI application. Examples of the Python Core API usage can be seen in:  
/home/vagrant/src/core/daemon/examples/netns


<b>SETTING CORE DEFAULT TERMINAL</b>
When CORE is running, the user can double click on a NEM to pull up an SSH session into the NEM.
By default, this terminal is set to gnome-terminal which is bugged in the current vagrant box.
In order to get the terminal session to work, inside the core-gui, change the default terminal 
to x-term. This option is found in Edit->Preferences->Terminal Program.

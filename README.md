# SIEM Integration with Artifical Intelligence(ChatGPT)


<h2>Description</h2>
In this project, we are going to create a customized ChatGPT integration in Azure Cloud, leveraging AI for advanced capabilities with security and event management systems.
<br />


<h2>Languages and Utilities Used</h2>

- <b>ChatGPT</b> 
- <b>Microsoft Sentinel</b>

<h2>Environments Used </h2>

- <b>Microsoft Azure</b> 

<h2>Walk-through:</h2>
We need to first create a free microsoft azure account with which you get your first $200(credits). You can follow the steps provided to create your first free Azure account by clicking on the link below:

#### [Azure Portal](https://azure.microsoft.com/en-us/free/)

We will be leveraging Microsoft Sentinel Playbooks which are a set of automated workflows that can be used to respond to security in Sentinel. These playbooks can be used to automate incident response, orchestrate actions and perform different security related tasks. 

To integrate ChatGPT into Sentinel and to create our first playbook, we need to search for Sentinel in search at Azure home page and create our workspace. Name the workspace something which you can remember easily. 

<p align="center">
<br/>
<img src="https://imgur.com/jxrmaTy.png" height="80%" width="80%" alt="Windows Azure Home"/>
<br />

Once you have create a workspace, add the created workspace to the Sentinel by clicking on "Add" button. This will add the Sentinel to the created workspace and once done, you can go ahead click on "Automation" to create our first playbook and to start integrating ChatGPT with Sentinel.

<p align="center">
<br/>
<img src="https://imgur.com/C7jECqy.png" height="80%" width="80%" alt="Windows Azure Home"/>
<br />

<br/>
<img src="https://imgur.com/HuCDADc.png" height="80%" width="80%" alt="Windows Azure Home"/>
<br />

Now we have to configure the virtual machine. Create a new resource group and name it something easy to remember. Choose the operating system as Debian since the honeypot requires it and remember that the region you choose will affect the cost of the machine you are deploying.

<p align="center">
<br/>
<img src="https://imgur.com/mRtqNqt.png" height="80%" width="80%" alt="Windows Azure Home"/>
<br />

There is an option called "Run with Azure Spot Discount" which you can choose. With this you can save on overall virtual instance cost but remember that with this option selected, Azure at any point can delete your machine. Since we don't wanna be interrupted with our installation process, we will keep this option disabled.

For VM size, choose the Standard_D4s_v3 as other sizes within the free credit range tend to be slow.

<p align="center">
<br/>
<img src="https://imgur.com/AJyo1bb.png" height="80%" width="80%" alt=""/>
<br />

To authenticate with your virtual machine, we can either choose password or SSH public key. We will be using password for our case. Make sure you save the password as we will be using it in the future.

In the inbound port section, make sure that the SSH is selected as we will be using it later to connect to our virtual machine. Now click on "Next:Disks >" to continue creating the disk size.

<p align="center">
<br/>
<img src="https://imgur.com/xzib8rB.png" height="80%" width="80%" alt=""/>
<br />

We will need to create at least 128 gigabyte disks, to do so, you will to have select "Create and attack a new disk" > "Change size" and select the 128GB disk option.

<p align="center">
<br/>
<img src="https://imgur.com/4hnohY2.png" height="80%" width="80%" alt=""/>
<br />

<br/>
<img src="https://imgur.com/SfaOwJ1.png" height="80%" width="80%" alt=""/>
<br />

Now we can move to the networking section. Click on "Next: Networking >" You will notice that a new virtual network subnet and public address has been created. No need to make any changes in networking section. Click on "Next: Management >". Here we have a very important functionality, which is to enable auto shutdown. This option will automatically daily stop your VM to save some cost. Since will be working on this virtual instance only for a short while, we will be not selecting this option.

<h3>Note</h3>
It's also important to decide how long you want to keep your VM. As you will be charged for stopped VM because you have a public IP address associated with it. When we are done working on this project and don't have free credits, make sure to delete all the resources to avoid any extra charges.

After making all the necessary changes, click on "Review + create" option at the bottom and click on "Create" to confirm and create your Azure virtual machine. The final step we need to do is to download the private key. This private key will be required later to connect to the VM and configure the honeypot.

<h3>Installation and Configuration of HoneyPot</h3>

We will have to open a few ports, but to make it even more simple, we will be opening all ports which allows all the incoming and outgoing connection to these ports. Follow these steps to configure the ports:

<p align="center">
<br/>
<img src="https://imgur.com/FaZprKC.png" height="80%" width="80%" alt=""/>
<br />

Go to "Networking" section and then to open ports, you simply add an inbound rule by clicking the "Add inbound rule" option. Since we are opening connection to all ports, we will specify the whole range from 0 - 65,535 ports. Set the action to allow to allow connections to these port range and then click on add. 
 
<p align="center">
<br/>
<img src="https://imgur.com/TL1OET1.png" height="80%" width="80%" alt=""/>
<br />

<br/>
<img src="https://imgur.com/Egmffos.png" height="80%" width="80%" alt=""/>
<br />

<h3>Downloading and Installing PuTTY</h3>

Now all that's left to do is to connect to our VM, and for for that we will use PuTTy. PuTTY is an SSH and telnet client, developed originally by Simon Tatham for the Windows platform. You can download PuTTY from it's official website and follow the basic installation wizard to complete the installation. Once you are done installing, you should see something like this. 

<p align="center">
<br/>
<img src="https://imgur.com/OU8yuA3.png" height="80%" width="80%" alt=""/>
<br />

To use PuTTY to connect to our VM, we need to provide the IP address of the VM, which you can find by clicking on the "Overview" section.

<p align="center">
<br/>
<img src="https://imgur.com/FxeqXqc.png" height="80%" width="80%" alt=""/>
<br />

Once you have the IP address of your VM, enter it in the PuTTY and click on "Open" to open a connection to our VM. Click on "Accept" when you see a prompt. And once the terminal is loaded, enter the username and password you had set up when you first created your VM.

<p align="center">
<br/>
<img src="https://imgur.com/EOIoBkQ.png" height="80%" width="80%" alt=""/>
<br />

We will be downloading and installing some updates. For that, we need to enter "sudo apt install" and "sudo apt upgrade -y" into the console of PuTTY.

Now, we need to download the Honeypot into our VM, for that we need install git into our VM. We will be using a Github repository to download the Honeypot and link to the repository is provided down below. 
#### [HoneyPot Repository](https://github.com/telekom-security/tpotce)

<p align="center">
<br/>
<img src="https://imgur.com/bY1iTqs.png" height="80%" width="80%" alt=""/>

To clone the repository, copy the command and paste it in the PuTTY terminal.

```javascript
git clone https://github.com/telekom-security/tpotce
```

To install the Honeypot we just cloned, we need to be in the right installer directory and enter the below commands to complete the installation.

```javascript
cd tpotce/iso/installer/
```
```javascript
sudo ./install.sh --type=user
```
This should start the installation process. Once you get to the prompt asking to continue, type "y" and press enter. You will be greeted with a blue screen asking to choose option to choose the T-Pot edition. Choose the "Standard" and we are good to go.

<p align="center">
<br/>
<img src="https://imgur.com/m70bWcq.png" height="80%" width="80%" alt=""/>
<br />

Once you have choosen the edition, the T-Pot will ask you to enter username and password. These credentials will be later used to log into T-Pot console and once you have provided the credentials, the rest of the installation will be done.

<h3>T-Pot Web Interface</h3>

Once the installation is done, the remote connection will be automatically terminated. We can close the PuTTY terminal and now to access the T-Pot web interface to track the cybersecurity threats on the Azure VM which we deployed, we need the VM's IP address and the T-Pot service runs on the port 64297. So to access web interface, copy the link below and paste it in your browser.

```javascript
https://[youripaddress]:64297
```
You will be prompted to enter the username and password. The credentials are the ones which we entered when first installed T-Pot clone from Github. Once you have entered the credentials, you will be greeted with the T-Pot web interface, where you can monitor the attacks that are performed on your VM. All the options available on the web interface have been detailed explained in the Git repository of T-Pot. So make sure to check it out.

<h3>Discovering Cyberthreats in Realtime with T-Pot</h3>

Once we have successfully logged into the T-Pot interface, let it run in the background for couple of hours(preferrably 1-2 days) so that potential attackers on the internet can access the honeypot and the T-Pot will gather the information for us to analyze.

<h4>Attack Map</h4>

Attack Map displays a live attack map that shows the geographic location of the attackers and the types of attacks that are being attempted on your honeypot.

<p align="center">
<br/>
<img src="https://imgur.com/pd7dTLC.png" height="80%" width="80%" alt=""/>
<br />

<br/>
<img src="https://imgur.com/wfSwctN.png" height="80%" width="80%" alt=""/>
<br />

With all the features provided by T-Pot you can do many more actions to better understand on how and which attacks are used by the attackers. For example, this is one of the dashboard you can use to visualize the attacks using Kibana.

<p align="center">
<br/>
<img src="https://imgur.com/jeY0r5U.png" height="80%" width="80%" alt=""/>
<br />

<br/>
<img src="https://imgur.com/UgxeBnL.png" height="80%" width="80%" alt=""/>
<br />

<h3>Note:</h3>
One more thing before we end the project, we need to delete the resource group where we have deployed our VM and the honeypot. That way we will avoid any charges from this project in the future. This step is pretty straight forward. Just select the resource group and select "Delete resource group" option to delete all the resources that are assigned to this VM.

<p align="center">
<br/>
<img src="https://imgur.com/xu7v00P.png" height="80%" width="80%" alt=""/>
<br />


And this finally concludes the project on creating a honeypot on Microsoft Azure and monitoring the honeypot using T-Pot.

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

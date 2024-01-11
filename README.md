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

Sentinel playbooks provides couple of options for us to choose, each providing an advantage. But for our purposes, we will be selecting "Playbook with Incident Trigger". This option will trigger automation when the incident is created. 

<p align="center">
<br/>
<img src="https://imgur.com/Bmxlz8N.png" height="80%" width="80%" alt="Windows Azure Home"/>
<br />

Here, we have to specify the name of the playbook and we can also enable diagnostic logs. Diagnostic logs will allow us to track any failures with the playbook.

<p align="center">
<br/>
<img src="https://imgur.com/bCRyTer.png" height="80%" width="80%" alt="Windows Azure Home"/>
<br />

Now in the Connection section, you will see a connection with managed identity to Sentinel. Sentinel will automatically create a managed identity for the playbook as a way to authenticate. This eliminates the need to store any explicit credentials or secrets in the playbook or manage them separately. We can click on "Review and create" to create our playbook. 

After creating the playbook, you will be redirected to Logic app designer window. Logic app and playbooks are basically synonyms. We call them playbooks in Microsoft Sentinel and they provide users with a graphical user interface to visualize and build workflows with different services and applications. The designer is structured from a different building blocks, each representing a specific action or connector that you can use to build your workflow. These building blocks include triggers, actions and conditions.

Let's move on and start working on our playbook. We will start by selecting a "New step" and there in the search bar, search for GPT and choose the "GPT3 Completes your Prompt". Don't forget to name your connection.

<p align="center">
<br/>
<img src="https://imgur.com/QPOCBRu.png" height="80%" width="80%" alt="Windows Azure Home"/>
<br />

<br/>
<img src="https://imgur.com/VKZv3Hc.png" height="80%" width="80%" alt="Windows Azure Home"/>
<br />

After this we will have to provide an API key. API key needs to start with bearer followed by space and your unique API key. We can get our API key from the official OpenAI website and you can visit the website by clicking on the link below.

#### [OpenAI Website](https://platform.openai.com/api-keys)

Now to create the API-key, click on "Create new secret key" and provide a name and a key will presented to you

<h4>Note:</h4>

Once you leave the website after generating the key, you won't be able to retrieve your API key again. So make sure you store it somewhere securely. 

Moving back to Azure, let's create the string that we're supposed to enter in the API-key section. Here's the format that you're supposed to enter in:

```javascript
Bearer [your api key]
```

<p align="center">
<br/>
<img src="https://imgur.com/42EFius.png" height="80%" width="80%" alt=""/>
<br />

Confirm with clicking on "Create" and you will be presented with options for your connection. For now, let's change the most important field which is "Prompt". We will be providing a question/prompt for ChatGPT, which can include questions like "How can I remediate" and here after we can put some dynamic content. Dynamic content refers to data that is generated or retrieved at runtime based on the context of the workflow. In this case, we will use our Sentinel incident building block.

As we can see, we are provided with mutliple options that can be used. For example, entities which could be a unique name, IP address, host and many more. And for our case, we are interested in "Incident title" followed by the "Incident description". Incident description is our final prompt. How can I remediate specific incident title will be put in here with a description of the incident description.

<p align="center">
<br/>
<img src="https://imgur.com/60VkQqV.png" height="80%" width="80%" alt=""/>
<br />

Finally, we want to put the response from the ChatGPT inside our Sentinel incident. Add another step and search for Add comment to incident

<p align="center">
<br/>
<img src="https://imgur.com/YBkZDOo.png" height="80%" width="80%" alt=""/>
<br />

Now we have to specify unique incident ID. We can find this in the dynamic context section. Just search for "incident ARM ID" and select the presented block. 

<p align="center">
<br/>
<img src="https://imgur.com/gnlYAy7.png" height="80%" width="80%" alt=""/>
<br />

Lastly, we will provide an incident comment message, which will be the output from the ChatGPT. Going back to the dynamic content in the GPT3 completes your prompt section, click on "See more" and from there select "Text". Once you click on it, a new building block will be created. That's because of the parameter that we have selected. This enables you to perform action for each individual item in a set of values based on the output parameters you have selected. This is how we have created our first playbook. Make sure you save it by click on "Save" option on the top. 

<p align="center">
<br/>
<img src="https://imgur.com/bJtCffc.png" height="80%" width="80%" alt=""/>
<br />

<br/>
<img src="https://imgur.com/P6UlSxw.png" height="80%" width="80%" alt=""/>
<br />

<h3>Assigning Permissions to ChatGPT</h3>

Now that our playbook is created, we need to assign appropriate privileges. If you want to use a playbook to write commands in Microsoft Sentinel, we will need to assign the Microsoft Sentinel responder role to that playbook. By assigning Sentinel Responder role to that playbook, we are allowing it to perform certain actions within Sentinel. This includes, creating and updating incidents, adding comments and running automation workflows. This ensures that your incident response process is streamlined and efficient while also maintaining security and compliance based on least privilege.

To assign Microsoft Sentinel responder role to a playbook, you need to navigate to the resource group where you have deployed the Sentinel solution. Once you are in the resouce group, click on "Access management(IAM)" option. Click on "Add" and select "Add role assignment".

<p align="center">
<br/>
<img src="https://imgur.com/INB5T20.png" height="80%" width="80%" alt=""/>
<br />

<br/>
<img src="https://imgur.com/hPcMe4t.png" height="80%" width="80%" alt=""/>
<br />

In the "Add role assignment" window search for Microsoft Sentinel and select Microsoft Sentinel Responder.

<p align="center>
<br/>
<img src="https://imgur.com/1Im6IpR.png" height="80%" width="80%" alt=""/>
<br />

Now, go to "Members" section and choose "Managed identity" option. Now click on "Select members" option and choose the "Logic app" option in Managed identity and select the playbook that we have created. After that, we can finally review and create to assign the Microsoft Sentinel Responder to our playbook.

<p align="center>
<br/>
<img src="https://imgur.com/8uuwNI0.png" height="80%" width="80%" alt=""/>
<br />

<h3>Running ChatGPT on Cybersecurity Incidents</h3>



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

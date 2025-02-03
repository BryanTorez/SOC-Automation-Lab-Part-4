<h1>SOC Automation (Home Lab) - Part 3</h1>

<p align="center">
<img src="https://snipboard.io/col95p.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h2>Description</h2>
<br />
<p align="center">
Welcome to part four of five on the series of the SOC automation project. If you haven't seen the previous parts where we go over how to build out a diagram for this lab, install what is required, and configure the components, I highly encourage you go and read that first to get up to speed.
<br />
<br />
<img src="https://snipboard.io/mLejC6.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Today's objective is to generate telemetry from our Windows 10 machine and making sure it is being ingested into Wazuh. By the end of this part, you would have properly configured, sent telemetry containing Mimikatz, and trigger a custom alert that you created. Let's get started.
<br />
<br />
<img src="https://snipboard.io/gNeChT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Starting with our Windows 10 machine. When we install Wazuh, the configuration for it will be under "programs file x86". So let's go ahead and open that up, and it will be under the file "ossec-agent". The file that we are most interested in is called "ossec.com".
<br />
<br />
<img src="https://snipboard.io/uSZU3a.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Y9JfUz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/ZE31pk.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
We can right-click this and open with notepad. Now let's just try opening this up in Notepad. This configuration will contain everything related to Wazuh.
<br />
<br />
<img src="https://snipboard.io/QVLbqJ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/tVoOye.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
If we scroll down just a bit, we will see what is called "Log Analysis" and under the "security" location, you notice how there are some event IDs that are being excluded by the "!=". For those that don't know, this means that it does not equal to.
<br />
<br />
<img src="https://snipboard.io/97isuP.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
If I wanted to monitor for Powershell for example, we can follow the same syntax as the local file. Just as this one right here. So I can copy this and then paste it right underneath, then type in "Powershell". However, because I'm not interested in Powershell and instead I want to look for processes that will contain Mikikatz.
<br />
<br />
<img src="https://snipboard.io/jEbcL6.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/nyz0aT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/qQ7dJv.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
To do that, we either should have Sysmon installed or enable Windows security event ID, '4688'. We will utilize the Sysmon method since we installed Sysmon in part two of this series. Let's configure our "ossec.conf" configuration file, to ask it to ingest our Sysmon logs. However, before we make any configurations. I'm going to close this out and not save.
<br />
<br />
<img src="https://snipboard.io/PSoXVt.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I'm going to copy out the "ossec.conf" file and then just make a backup of it, just in case we mess something up. So I'll rename this file as "ossec-backup" because again, if we make a mistake we can just revert it.
<br />
<br />
<img src="https://snipboard.io/WSAPtT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/i9JUGp.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now let's go ahead and double-click the configuration file and scroll down to "Log Analysis". I'll go ahead and start copying one of the local file tags and right underneath "Application", I'll just paste it in.
<br />
<br />
<img src="https://snipboard.io/7FnXKT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Hvlzcx.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/dWUiTJ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Currently for the location name, it is set to "Application". Since I want to ingest Sysmon logs, let's change this to Symon's channel name. Now, how do we get that channel name? Well we can get it through opening up "Windows Event Viewer". We'll expand "Applications and Services". Then, expand "Microsoft Windows".
<br />
<br />
<img src="https://snipboard.io/RMH9xG.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/O9afkT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Scroll down until you see "Sysmon". Now, I'll expand "Sysmon" and right-click "Operational". Select "Properties" and from here, we can see the full name. This is going to be the name that we will be using. So I'll go ahead and copy that and head back over to our configuration file. I'll paste it into this "Application" location.
<br />
<br />
<img src="https://snipboard.io/tGk51m.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/6vK3r7.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/NrKA9W.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/TpW2Nq.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, if I wanted to ingest Powershell, instead of Sysmon. Scroll up until you find "PowerShell" and then I'll right click "Operational". Hit "Properties". This is the name that I'll copy and paste into the "location" tag.
<br />
<br />
<img src="https://snipboard.io/GM6Ks0.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/AXuPpi.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/LSkCn8.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
For the sake of ingestion, I'm going to remove the local file "Application". I'm also going to remove the "Security" and "System" and then I'll just leave the "Active Response" here. 
<br />
<br />
<img src="https://snipboard.io/SE8mxQ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/l1DrzI.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/4lwDxT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/NyvqdE.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, in other words this means that "Application", "Security", and "System" will no longer forward events to our Wazuh manager. If you wanted to forward those logs, you can keep them as is but just for an ingestion's sake, I only care about Sysmon. So we will go ahead and save this file out. Then, I'll replace it. Here it says, "You do not have permission to open this file". Hit "OK". 
<br />
<br />
<img src="https://snipboard.io/L0asYN.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/CGuESJ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So we do need administrative permissions, that's what I thought. So we'll type in "Notepad" and then I'll right-click it and "Run as administrator". I'll hit "Yes". Now from here, I'm just going to copy out the "Sysmon" location tag.
<br />
<br />
<img src="https://snipboard.io/iSMjbp.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Vcv0Xz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/l1XY0U.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I'll click on "File". Click "Open" so we can open up the "ossec" configuration file. Again, I'm going to remove "Application", "Security", and "System". Then, I'll paste in my Sysmon and now we will go ahead and save this file.
<br />
<br />
<img src="https://snipboard.io/S3zf7R.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/zS7u91.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/y0K3qX.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/tgOh0R.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
The next step is to open up our "Services" and restart our Wazuh Service. As a side note, anytime you change your configurations you must restart the service. Head over to our Wazuh dashboard and click "Events". Make sure that we're in the alerts index so we can start searching for Sysmon events.
<br />
<br />
<img src="https://snipboard.io/uens98.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/6zGQ7E.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/wYTzlD.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/kbYRK0.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
It might take some time until you can actually search for some Sysmon events, so if you do not see it at first that is okay. The next thing we want to do is download Mimikatz onto our machine. We want to make sure that we disable Windows Defender or at least exclude our "Downloads" folder.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Since it will detect it for sure. For those that don't know what Mimikatz is or does, it is an application that attackers and red teamers use to extract credentials from your machine. Before I download Mimikatz, I will go and exclude the downloads path.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So I'll type in "Security" and select Windows Security. I'll dismiss the "Virus and threat protection". Under "Manage Settings" for Virus and threat protection settings, you want to scroll down and click on "Add or remove exclusions". I'll add an exclusion. 
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I'll add it as a folder and then I'll select my "Downloads" folder to exclude. Select "Yes". Now our downloads folder is excluded. I'll go ahead and download Mimikatz and save it in my downloads folder.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
If you are downloading Mimikatz, your web browser might block that. Now if you're using Google Chrome like I am, go into your Google Chrome settings and under "Privacy and Security" you want to select "Security".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Scroll down and select "No protection". Turn that off and now you can try and download Mimikatz. Once Mimikatz is downloaded, I'll right-click it and select "Extract all". Now I have Mimikatz here.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So what I'll do is open up an administrative Powershell session and then change my directory into Mimiktz. Now I will run ".\mimikatz.exe" and now we'll head back over to Wazuh's dashboard and check if you can see any events related to Mimikatz.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So I'll type in "mimikatz and hit "Enter". Now, you might not see any events relating to Sysmon or even Mimikatz. That could be because due to the fact that your Syssmon events did not trigger any alerts or rules from Wazuh. Since by default, Wazuh does not log everything and only logs things when a rule or alert is triggered.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Of course, we can change this behavior by going into the Wazuh Manager and configuring the "ossec.com" file. To make it so, it logs everything or we can create a rule that looks at specific events. That way when a particular event does exist, it will trigger an alert in Wazuh and then we can search for it.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So why don't we go and do that. Let's go and modify the "ossec" configuration to log everything. In Wazuh manager's CLI, the first thing we want to do is make sure to create a backup of the "ossec.conf" file and that is located in "cp /var/ossec/etc/ossec.conf ~/ossec-backup".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Again, mistakes do happen and if we accidentally overwrite something, we can always revert it back using our backup file. Now let's go ahead and edit that file. So I'll open up the "ossec.com" file and hit "Enter".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, if you look at the bottom-right under "alerts_log" you see a "logall" section and a "logall_json" section. Currently, they are both set to "no". So I'll change this to "yes". Now, essentially this is asking what format do you want the logs to be displayed in.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I'll save this and hit "yes". Now let's restart the Wazuh manager by typing in "systemctl restart wazuh-manager.service" and then you can hit "Tab" for auto-completion. Now what this does is force Wazuh to begin archiving all the logs and put them into a file called "Archives".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
This file will be located in "cd /var/ossec/logs/archives". Type in "ls". These are the files that will be created and the logs will be placed in here. In order for Wazuh to start ingesting these logs, we need to change our configuration in "filebeat".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
To do that, we type in "nano /etc/filebeat/filebeat.yml". If we were to scroll down, we'll eventually see a setting called "archives: enabled: false". So let's change this to true and then we will save that out.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Just like anything, if you update a configuration you must restart its service. So I'll restart filebeat's service. Now that we've updated filebeat, as well as the ossec configuration. Let's head back over to our Wazuh dashboard and create a new index
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
On our dashboard, click the hamburger icon at the top left. You want to scroll down to select "Stack Management". Click on "Index Patterns". Then, you'll notice how we have three indexes so far. We have "alerts", "monitoring", and "statistics".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
We want to create an index for archives. That way we can search all the logs, regardless if Wazuh triggered an alert. To do that, we can click on "Create index" at the top right corner so we can name our index here. So I'll type in "Wazuh-archives-**" for everything.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I'll click on "Next" and for the time field, I'll select "timestamp". Then, select "Create index pattern". Now, let's head back over to "Discover". We can head over there by clicking on the top left hamburger icon and then "Discover".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Select the indexes from the drop-down arrow and then we want to select our archives. Now, it might take a while until events start to flow in, but it will come in eventually. One thing you can do to troubleshoot is cat out the archive file and Mimikatz.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
If you see it in archives, it will be ingested in the dashboard. It just takes some time. 
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, Wazuh is a bit special. By default, not all logs will show in the manager. Only those that trigger a rule will show up. That is why we had to configure the logs to log everything, making it so regardless of a rule being triggered or not, we want the manager to archive it and allow us to search for them.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, don't get me wrong. Although what they are doing by default is great, but if we are testing, it hinders our test. Do keep this in mind, if you do play around with Wazuh. So in our Wazuh manager CLI, under the following directory path "/var/ossec/logs/archives".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
If we were to type in "ls", we can see "archives.json" and "archives.log". We definitely see that there are events in those files. If you want to troubleshoot, you can cat out the archive file. So I will cat out "archive.json" and then I'll pipe it into a "grep -i" for ignore case sensitivity.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Then, I'll search for "mimikatz". Now if I don't see anything in my archives, then a Mimikatz event did not generate. So this means that no matter what, I'm not going to see Mimikatz in my Wasa dashboard. If I don't see any Mimikatz events, the next thing that we can do is try and regenerate it.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Let's type in "mimikats" again. Hit "Enter". Open up our "Event Viewer" just to make sure that Sysmon is capturing Mimikats. The event ID that I'm most interested in is "Event ID:1" because these are process creations. Here I can see that there is in fact Mimikats.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So Sysmon is generating on my Windows machine and we did configure our ossec configuration to push Sysmon data over to Wazuh. The next thing we can do is, check our Wazuh manager again and grep for Mimikats. We actually see some data regarding Mimikats in our archive file.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
This means that if we search for Mimikats in our Wazuh dashboard and we do not see any events yet, we just need to be a little patient and eventually it will get in there. Let's head over to our dashboard and take a look.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I am under the archives index because that is what we want and then I will type in "mimikatz" and hit "Enter". We have two events, that's perfect. Now remember, we are interested in "event ID:1" because this will show us process creations.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
If you don't see Mimikats in Wazuh, but you see it in your "archive" log file, you can try and force the ingestion by restarting your Wazuh manager service. Of course, you don't want to do that in production, but because it's demo it's fine.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, if I were to expand the "event ID:1" and scroll down a bit, we can take a look at the fields. I noticed that there is a field called "OriginalFileName". We will use this field to craft our alert, because if we were to use a field such as "image".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
An attacker could simply rename "Mimikats to "Mimikow" and the alert would have been bypassed. However, with the "OriginalFileName" regardless of the name change to "Mimikcow", we should still be able to see it and the alert should trigger.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now let's start creating our alert. Wazuh has some built-in rules that we can use as a reference stored in the location "/var/ossec/ruleset/rules" and it's located in the Wazuh manager CLI. However, the good news is that we can actually access this on the dashboard itself.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So if you don't like playing around with CLI, this is great news for you. To access the rules, you want to click on the home button. Then, there will be a drop-down next to it. Just go ahead and click on that and then select "Management".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
You'll see rules over here. Click on "Rules" and now you want to click on "Manage rules files" at the top right. Since we're interested specifically in the "event ID:1" for Sysmon, let's try and find that by typing in "sysmon". Hit "Enter".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now immediately I can see there is a "0800-sysmon_id_1". We can take a look at it by clicking on this eye icon. These are Sysmon rules that are built into Wazuh specifically targeting "event ID:1". 
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I'll copy one of these for reference and build it out as a custom rule to detect MimiKatz. So let's just select the first one here. I'll select the "rule id" all the way to "rule" and then copy that out.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Once we copy that, head back and click on "Custom Rules". I'm going to minimize the browser, because you cannot see it, but there's a custom rules button here. I'll go ahead and click on that and immediately we see one "local_rule" file.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
We can edit that by clicking on the pencil icon. We want to paste in the rule that we copied into the "local_rule" that already exists, but you want to do that below the rule.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So one thing to keep in mind is the indentation. I would recommend you follow the same as the rule above and they do use spaces. Now, to begin customizing this rule, we'll start with the ID field. Custom rules should always start from '100,000'.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
If you look above the rule ID, its '100,001' so I cannot use that. Instead, I'm going to put this as '100,002'. The level is the severity, so the higher the level, the higher the importance is.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
The highest level you can add is up to '15'. So for fun, let's just put it as '15'. I'll change '4' to '15'. For the field name, we know that we want "OriginalFileName". So currently, it is looking at "parentImage".
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I'll remove this and type in "OriginalFileName". One thing to keep in mind is case sensitivity. You want to make sure it matches exactly the field name. So for example, I have a capital "F" and "N", if I did not capitalize the "F" and then just put it as lowercase "fileName", this alert will never trigger because there's no field name with that.
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
The field name that exists has a capital f for file so do keep that in mind when you're creating an alert case sensitivity matters as for the type the PC2 we'll leave it as is it is essentially Rex the bracket question mark I is set to ignore case sensitivity for the value not the field name but the field value so right now it is looking for C or W script .exe I'm just going to remove that and then type in mimicat that way it's searching specifically for mimic cats in the original file name right underneath it options no full log I am going to remove this because I want all the logs for the description it's right now it is scripting interpreter spawn I am going to change this to let's say mimik cats use message detected and for the miter ID I'm going to remove this and type in 03 because that is credential dumping which is what mimik cats is known to do and again just make sure the indentation is the same as the one above and it does use spaces so let's go ahead and save this out and then we can restart the manager once you save it it would automatically tell you to restart and there's a button for for you to do that so I will click on confirm that the manager will be restarted before we run mimik cats just to prove this point I'm going to change its file name and maybe name it as you are awesome exe or actually I'll remove the exe because that is already implied there and then I'll just name it as you are awesome now I'll open up my Powershell exit out mimik cats and actually before I even do all that let's just head over to Security events and double check just to make sure that there's no alert for mimik cats currently there's none so that's perfect so let's head over to our Powershell and now we will type in you are awesome. exe and then I'll hit enter back onto our was dashboard let's go and refresh this scroll all the way down and there you go perfect perfect we even used our renamed mimic cats and the alert still triggered because we are looking at the original file name instead if we were looking at the image the mimik cats executable have been renamed to you are awesome. exe so we were monitoring for an alert specifically for mimik cats as an image field value then our alert would not trigger that is why we should look at the original file name and right here it says Mim cats we did a lot in this video I showed you how to create an alert and how we can customize our Wasa configuration to ingest certain logs in this example we configured cismon to be ingested into Wasa if you are a student or a professional that wants to transition into cyber security I want you to know that I offer free mentorship on my site with no strings attached on there you will also see products that I've personally created in which you can download to help guide you along this journey these products include resume and cover letter templates bookmarks a one-year road map on how to get started in cyber security and a list of interview questions to help you in your next interview also as a sneak peek I am in the process of creating a sock course where there will be over 20 Hands-On labs and multiple projects that you can put onto your resume you can join the wait list if if you choose to do so my mission here is to help you get to where you want to be if you ever have a question about a certain field or what a certain function does I do encourage you to stop and research what that is and learn more about it now in the final episode we will begin performing some automation using Shuffle and the hive and that is it for the video and I hope you found it informative if you do happen to have any questions remember try and research it to find the answer if you can't find the answer leave it in the comment section down below I'll try my best to help you out remember to stay curious and do things differently

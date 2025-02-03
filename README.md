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
<img src="https://snipboard.io/5UhQkN.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Since it will detect it for sure. For those that don't know what Mimikatz is or does, it is an application that attackers and red teamers use to extract credentials from your machine. Before I download Mimikatz, I will go and exclude the download path. So I'll type in "Security" and select Windows Security. 
<br />
<br />
<img src="https://snipboard.io/kf57Qu.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
I'll dismiss the "Virus and threat protection". Under "Manage Settings" for Virus and threat protection settings, you want to scroll down and click on "Add or remove exclusions".
<br />
<br />
<img src="https://snipboard.io/m1oi72.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/SCT6Ia.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/7m6XBR.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/1zXMi8.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I'll add an exclusion. I'll add it as a folder and then I'll select my "Downloads" folder to exclude. Now our downloads folder is excluded. I'll go ahead and download Mimikatz and save it in my downloads folder.
<br />
<br />
<img src="https://snipboard.io/3BhicF.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/zIMrvo.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/vXhSgx.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/2G9Y3K.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
If you are downloading Mimikatz, your web browser might block that. Now if you're using Google Chrome like I am, go into your Google Chrome settings and under "Privacy and Security" you want to select "Security".
<br />
<br />
<img src="https://snipboard.io/YAT7Qk.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/PeQLVl.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Scroll down and select "No protection". Turn that off and now you can try and download Mimikatz. Once Mimikatz is downloaded, I'll right-click it and select "Extract all". Now I have Mimikatz here.
<br />
<br />
<img src="https://snipboard.io/DNIpgx.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/wGVUM0.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/6MZEXQ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So what I'll do is open up an administrative Powershell session and then change my directory into Mimiktz. Now I will run ".\mimikatz.exe" and now we'll head back over to Wazuh's dashboard and check if you can see any events related to Mimikatz.
<br />
<br />
<img src="https://snipboard.io/iBqzxS.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/TlLdXE.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/0Ms64X.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/d1HSTQ.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So I'll type in "mimikatz and hit "Enter". Now, you might not see any events relating to Sysmon or even Mimikatz. That could be because due to the fact that your Sysmon events did not trigger any alerts or rules from Wazuh. Since by default, Wazuh does not log everything and only logs things when a rule or alert is triggered. Of course, we can change this behavior by going into the Wazuh Manager and configuring the "ossec.com" file. To make it so, it logs everything or we can create a rule that looks at specific events. That way when a particular event does exist, it will trigger an alert in Wazuh and then we can search for it. So why don't we go and do that. 
<br />
<br />
<img src="https://snipboard.io/iNj3D1.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/Fsx5la.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Let's go and modify the "ossec" configuration to log everything. In Wazuh manager's CLI, the first thing we want to do is make sure to create a backup of the "ossec.conf" file and that is located in "cp /var/ossec/etc/ossec.conf ~/ossec-backup".
<br />
<br />
<img src="https://snipboard.io/BY5dP7.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Again, mistakes do happen and if we accidentally overwrite something, we can always revert it back using our backup file. Now let's go ahead and edit that file. So I'll open up the "ossec.com" file and hit "Enter".
<br />
<br />
<img src="https://snipboard.io/kQY914.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, if you look at the bottom-right under "alerts_log" you see a "logall" section and a "logall_json" section. Currently, they are both set to "no". So I'll change this to "yes". Now, essentially this is asking what format do you want the logs to be displayed in.
<br />
<br />
<img src="https://snipboard.io/ZvomVq.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/EZqDQ1.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I'll save this and hit "yes". Now let's restart the Wazuh manager by typing in "systemctl restart wazuh-manager.service" and then you can hit "Tab" for auto-completion. Now what this does is force Wazuh to begin archiving all the logs and put them into a file called "Archives".
<br />
<br />
<img src="https://snipboard.io/ASqijX.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/ZsTaNk.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
This file will be located in "cd /var/ossec/logs/archives". Type in "ls". These are the files that will be created and the logs will be placed in here. In order for Wazuh to start ingesting these logs, we need to change our configuration in "filebeat".
<br />
<br />
<img src="https://snipboard.io/MWmpVK.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
To do that, we type in "nano /etc/filebeat/filebeat.yml". If we were to scroll down, we'll eventually see a setting called "archives: enabled: false". So let's change this to true and then we will save that out.
<br />
<br />
<img src="https://snipboard.io/8VdX45.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/hXuyN1.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/z1wkW7.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Just like anything, if you update a configuration you must restart its service. So I'll restart filebeat's service. Now that we've updated filebeat, as well as the ossec configuration. Let's head back over to our Wazuh dashboard and create a new index.
<br />
<br />
<img src="https://snipboard.io/YnyHAR.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
On our dashboard, click the hamburger icon at the top left. You want to scroll down to select "Stack Management". Click on "Index Patterns". Then, you'll notice how we have three indexes so far. We have "alerts", "monitoring", and "statistics".
<br />
<br />
<img src="https://snipboard.io/jNZPqz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/j0Iuox.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/hXtziN.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/tlLMKc.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
We want to create an index for archives. That way we can search all the logs, regardless if Wazuh triggered an alert. To do that, we can click on "Create index" at the top right corner so we can name our index. So I'll type in "Wazuh-archives-**" for everything.
<br />
<br />
<img src="https://snipboard.io/ABrp9o.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I'll click on "Next" and for the time field, I'll select "timestamp". Then, select "Create index pattern". Now, let's head back over to "Discover". We can head over there by clicking on the top left hamburger icon and then "Discover".
<br />
<br />
<img src="https://snipboard.io/8hgzow.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/rZTjBV.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/SayWcz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Select the indexes from the drop-down arrow and then we want to select our archives. Now, it might take a while until events start to flow in, but it will come in eventually. One thing you can do to troubleshoot is cat out the archive file and Mimikatz.If you see it in archives, it will be ingested in the dashboard. It just takes some time. 
<br />
<br />
<img src="https://snipboard.io/LG7tOH.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/5O0LP3.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, Wazuh is a bit special. By default, not all logs will show in the manager. Only those that trigger a rule will show up. That is why we had to configure the logs to log everything, making it so regardless of a rule being triggered or not, we want the manager to archive it and allow us to search for them. Now, don't get me wrong. Although what they are doing by default is great, but if we are testing, it hinders our test. Do keep this in mind, if you do play around with Wazuh. 
<br />
<br />
<br />
<br />
So in our Wazuh manager CLI, under the following directory path "/var/ossec/logs/archives". If we were to type in "ls", we can see "archives.json" and "archives.log". We definitely see that there are events in those files. If you want to troubleshoot, you can cat out the archive file. So I will cat out "archive.json" and then I'll pipe it into a "grep -i" for ignore case sensitivity. Then, I'll search for "mimikatz". 
<br />
<br />
<img src="https://snipboard.io/BvfjJs.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/NowEYV.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/D5WQM1.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now if I don't see anything in my archives, then a Mimikatz event did not generate. So this means that no matter what, I'm not going to see Mimikatz in my Wazuh dashboard. If I don't see any Mimikatz events, the next thing that we can do is try and regenerate it.
<br />
<br />
<img src="https://snipboard.io/BIYMzi.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/E8eYap.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Let's type in "mimikats" again. Hit "Enter". Open up our "Event Viewer" just to make sure that Sysmon is capturing Mimikats. The event ID that I'm most interested in is "Event ID:1" because these are process creations. Here I can see that there is in fact Mimikats.
<br />
<br />
<img src="https://snipboard.io/0qjbum.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/ctHKQ4.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/EYUGLT.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/McH69U.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
So Sysmon is generating on my Windows machine and we did configure our ossec configuration to push Sysmon data over to Wazuh. The next thing we can do is check our Wazuh manager again and grep for Mimikats. We actually see some data regarding Mimikats in our archive file. This means that if we search for Mimikats in our Wazuh dashboard and we do not see any events yet, we just need to be a little patient and eventually it will get in there. Let's head over to our dashboard and take a look.
<br />
<br />
<img src="https://snipboard.io/Gc6p5n.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/8j915S.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I am under the archives index because that is what we want and then I will type in "mimikatz" and hit "Enter". We have two events, that's perfect. Now remember, we are interested in "event ID:1" because this will show us process creations. If you don't see Mimikats in Wazuh, but you see it in your "archive" log file, you can try and force the ingestion by restarting your Wazuh manager service. Of course, you don't want to do that in production, but because it's demo it's fine.
<br />
<br />
<img src="https://snipboard.io/BLiM2p.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/h92YZy.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/4sgnPw.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, if I were to expand the "event ID:1" and scroll down a bit, we can take a look at the fields. I noticed that there is a field called "OriginalFileName". We will use this field to craft our alert, because if we were to use a field such as "image". An attacker could simply rename "Mimikats to "Mimicow" and the alert would have been bypassed. However, with the "OriginalFileName" regardless of the name change to "Mimikcow", we should still be able to see it and the alert should trigger.
<br />
<br />
<img src="https://snipboard.io/wYic4J.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/prkwi8.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now let's start creating our alert. Wazuh has some built-in rules that we can use as a reference stored in the location "/var/ossec/ruleset/rules" and it's located in the Wazuh manager CLI. However, the good news is that we can actually access this on the dashboard itself. So if you don't like playing around with CLI, this is great news for you.
<br />
<br />
<br />
<br />
To access the rules, you want to click on the home button. Then, there will be a drop-down next to it. Just go ahead and click on that and then select "Management".
<br />
<br />
<img src="https://snipboard.io/tDBSwL.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/NGzMmh.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
You'll see rules over here. Click on "Rules" and now you want to click on "Manage rules files" at the top right. Since we're interested specifically in the "event ID:1" for Sysmon, let's try and find that by typing in "sysmon". Hit "Enter".
<br />
<br />
<img src="https://snipboard.io/kdFMgt.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/yVJ1XO.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/bL2SBz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now immediately I can see there is a "0800-sysmon_id_1". We can take a look at it by clicking on this eye icon. These are Sysmon rules that are built into Wazuh specifically targeting "event ID:1". 
<br />
<br />
<img src="https://snipboard.io/9wk0Vz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/XLs1Sz.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/yjDrBO.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
I'll copy one of these for reference and build it out as a custom rule to detect MimiKatz. So let's just select the first one here. I'll select the "rule id" all the way to "rule" and then copy that out.
<br />
<br />
<img src="https://snipboard.io/NMEZLr.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Once we copy that, head back and click on "Custom Rules". I'll go ahead and click on that and immediately we see one "local_rule" file.
<br />
<br />
<img src="https://snipboard.io/AFzaKl.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/RA80tE.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
We can edit that by clicking on the pencil icon. We want to paste in the rule that we copied into the "local_rule" that already exists, but you want to do that below the rule. So one thing to keep in mind is the indentation. I would recommend you follow the same as the rule above and they do use spaces. 
<br />
<br />
<img src="https://snipboard.io/2chQTr.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/fiyHO9.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/YrLOaV.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Now, to begin customizing this rule, we'll start with the ID field. Custom rules should always start from '100,000'. If you look above the rule ID, its '100,001' so I cannot use that. Instead, I'm going to put this as '100,002'. The level is the severity, so the higher the level, the higher the importance is.
<br />
<br />
<img src="https://snipboard.io/iAanZm.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/zr4OLj.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
The highest level you can add is up to '15'. So for fun, let's just put it as '15'. I'll change '4' to '15'. For the field name, we know that we want "OriginalFileName". So currently, it is looking at "parentImage". I'll remove this and type in "OriginalFileName". 
<br />
<br />
<img src="https://snipboard.io/dmZehN.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<img src="https://snipboard.io/HRyYcl.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
One thing to keep in mind is case sensitivity. You want to make sure it matches exactly the field name. So for example, I have a capital 'F' and 'N', if I did not capitalize the "F" and then just put it as lowercase "fileName", this alert will never trigger because there's no field name with that. The field name that exists has a capital 'f' for file
<br />
<br />
<br />
<br />
So do keep that in mind when you're creating an alert, "Case sensitivity matters". As for the type, the "pcre2", we'll leave as it is. It is essentially regex. The "(?i) is set to ignore case sensitivity for the field value, not the field.
  
So right now, it is looking for 'C' or 'W' "script.exe". I'm just going to remove that and then type in "mimikatz". That way it's searching specifically for Mimikatz in the original file name.

Right underneath, where it says "options" and "no_full_log". I am going to remove this because I want all the logs. For the description, right now it is a scripting interpreter spawn. I'll go ahead and change this to "Mimikatz Usage Detected". For the mitre ID, I'm going to remove this and type in '03' since that is credential dumping, which is what Mimikatz is known to do.

Again, just make sure the indentation is the same as the one above and it does use spaces. Let's rename Mimkatz to "youareawesome". Then let's go ahead and save this out and then we can restart the manager. Once you save, it will automatically tell you to restart and there's a button for you to do that. 

Let's just head over to "Security Events" and double-check just to make sure that there's no alert for Mimikatz. Currently, there's none so that's perfect. So let's head over to our Powershell and now we will type in ".\youareawesome.exe". I'll hit "Enter". 

Now back onto our Wazuh dashboard, let's go and refresh this. Scroll down and there you go. Perfect. We even used our renamed "MimiKatz" and the alert is still triggered because we are looking at the original file name instead. If we were looking at the image, the Mimikatz executable would have been renamed to "youareawesome.exe". So we were monitoring for an alert specifically for Mimikatz as an image field value then our alert would not trigger. That is why we should look at the original file name and right here it says "mimkatz.exe".

We did a lot in this series. I showed you how to create an alert and how we can customize our Wazuh configuration to ingest certain logs. In this example, we configured Sysmon to be ingested into Wazuh. Now in the final episode, we will begin performing some automation using Shuffle and the Hive. Remember to stay curious and do things differently.

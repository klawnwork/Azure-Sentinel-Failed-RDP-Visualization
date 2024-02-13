<h1>Azure Sentinel Failed RDP Visualization</h1>


<h2>Description</h2>
Project consists of the automated gathering of failed log in attempts of a VM and processing/visualizing that data in a SIEM. 
<br />


<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 
- <b>Microsoft Azure</b>
- <b>Kusto Query Language</b> 
- <b>Remote Desktop Connection</b>

<h2>Environments Used </h2>

- <b>Windows 10</b> 

<h2>Program walk-through:</h2>


<p align="center">
Launch a Windows 10 Virtual Machine (VM) in Azure: <br/>
<br />
<img src="https://i.imgur.com/5Hv52Zt.png" height="80%" width="80%"/>
<br />
<br />
In the Networking section of the VM configuration, we select Advanced under NIC Network Security Group and set Destination Port Ranges to all ports, making our VM open to all traffic: <br/>
<br />
<img src="https://i.imgur.com/HTxpeKd.png" height="80%" width="80%"/>
<br />
<br />
Next, we create a Log Analytics Workspace (LAW) to ingest logs from the VM: <br/>
<br />
<img src="https://i.imgur.com/YUiVqlL.png" height="80%" width="80%"/>
<br />
<br />
In Microsoft Defender for Cloud, we go to Environment Settings and turn on Microsoft Defender for our LAW, ensuring security: <br/>
<br />
<img src="(https://i.imgur.com/XSxAymW.png" height="80%" width="80%"/>
<br />
<br />
Under the Data Collection tab, select All Events, which allows the LAW to gather logs from the VM: <br/>
<br />
<img src="https://i.imgur.com/mWOTXPc.png" height="80%" width="80%"/>
<br />
<br />
Then we connect the LAW to the VM: <br/>
<br />
<img src="https://i.imgur.com/OPFPHkh.png" height="80%" width="80%"/>
<br />
<br />
Now go to Microsoft Sentinel, the SIEM we'll use to organize and visualize the data. We add Sentinel to the LAW: <br/>
<br />
<img src="https://i.imgur.com/3kM9nyw.png height="80%" width="80%"/>
<br />
<br />
With the main infrastructure set up we can use the public IP address of out VM to connect to it via Remote Desktop Connection: <br/>
<br />
<img src="https://i.imgur.com/XlEfuW8.png" height="80%" width="80%"/>
<br />
<br />
Now in our VM, open Event Viewer to view security events. I purposefully tried to log in with invalid credentials, and we can see my information, including the Source Network Address, which will be used by the API to obtain geodata: <br/>
<br />
<img src="https://i.imgur.com/b28muEU.png" height="80%" width="80%"/>
<br />
<br />
This is ipgeolocation.io, which allows us to view geodata based on an IP address. I put in my own Network Address as an example, and we can see the Country, City, Latitude, Longitude, ect: <br/>
<br />
<img src="https://i.imgur.com/He4TGTl.png" height="80%" width="80%"/>
<br />
<br />
We are about to run our script, but before we do that we go to Windows Defender Firewall to disable the firewall for all profiles in our VM: <br/>
<br />
<img src="https://i.imgur.com/bQG3Uiz.png" height="80%" width="80%"/>
<br />
<br />
Now we are ready to run the script, and we get our personal API key from ipgeolocation.io to add to the script, which is visible in line 2. The script runs in perpetuity, continually scanning Event Viewer for failed log in attempts, grabbing the Source Network Address for those attempts, sending it to ipgeolocation.io via the API to obtain geodata, and then storing the geodata in a log file named failed_rdp.log : <br/>
<br />
<img src="https://i.imgur.com/Tldeqyv.png" height="80%" width="80%"/>
<br />
<br />
Next, we create a Log Analytics Workspace (LAW) to ingest logs from the VM: <br/>
<br />
<img src="https://i.imgur.com/YUiVqlL.png" height="80%" width="80%"/>
<br />
<br />

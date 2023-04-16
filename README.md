# Logging-and-Monitoring

In this lab we will use 2 GeoIP files which will help us correlate IP addresses to figure out where the attacks originated from. 
This will be interesting because in Event Viewer we only see IP addresses, but we have no clue where the attacks came from. 


We will first download 2 GeoIP files 
![image](https://user-images.githubusercontent.com/112146207/231026643-50acaa3b-3f83-4467-b6ee-49cf28ee2225.png)

We will then go into our Azure account and search "storage account" and click on "create storage account"

![image](https://user-images.githubusercontent.com/112146207/231028070-1e083867-7a13-4d6f-b4d7-a68c387c0ee5.png)

When we create our storage account make sure to put it under the "RG-Cyber-Lab" resource group, make a storage account name, change the region, and change the redundancy to locally-redundant storage (LRS).

Remember that Redundancy in cybersecurity is often used to improve fault tolerance, which means the ability of a system to continue functioning even if a part of it fails.

![image](https://user-images.githubusercontent.com/112146207/231028783-ea7691e8-3f53-4533-852d-4e86d36b566c.png)


Once the storage account is created, go to the search bar and type in "storage account" 
Within the storage account, create a container named “ipgeodata”

![image](https://user-images.githubusercontent.com/112146207/231030560-acfa43e5-4d3e-4319-becb-5e9bf0dd42df.png)

Click on "ipgeodata" and we will upload the 2 files we downloaded
This might take a while since one of the files is very large, but patience is key! 

![image](https://user-images.githubusercontent.com/112146207/231031297-24b3618d-59af-49ae-b564-7397895b5cb9.png)

We now need to create SAS URLs for both of these files.
A SAS is used to provide access to files on an individual basis instead of opening up the whole container

We will first copy the file names and jot it down on a notepad 

![image](https://user-images.githubusercontent.com/112146207/231032762-02446bd7-f2ae-4378-acea-31c8029c4083.png)

Right click the file name and press "generate SAS" and make sure it is the city block IPv4 

![image](https://user-images.githubusercontent.com/112146207/231033022-3a06ba97-7561-408a-9fb0-cd340e02744d.png)

Change the experation date and click "generate SAS" then copy the Blob SAS URL and put it on your notes because we will use it later

![image](https://user-images.githubusercontent.com/112146207/231033594-d39d112c-3a55-4bc2-9a05-e3336d00f910.png)

The same steps will be done for the "city-locations" file

![image](https://user-images.githubusercontent.com/112146207/231034805-cc50ffd3-0c0b-4862-959b-ec9c1275a094.png)

It's very important that you copied the Blob URLs to a notepad because we will use them

![image](https://user-images.githubusercontent.com/112146207/231035113-37153ffd-7b20-4039-89e0-32fa03063cd6.png)

---

Our SIEM will look into our work analytics workspace where it will collect, analyze and identify logs in real time. 

Go to portal.azure.com and search "log analytics workspace" then click "create"

Enter in your resource group, name, and region
hit "create"

![image](https://user-images.githubusercontent.com/112146207/231041157-07ff8690-0e94-4d1a-8855-b0a13f2ae6ca.png)

---

We just created our work analytics workspace which will be injected with Geo data to let us correlate IP addresses and see people's origins. We will then create our SIEMs resource and connect it to the log analytics workspace. 

Go to the search bar and search "Microsoft Sentinel" and hit "create" 

Then click on your workspace and click "add" 

![image](https://user-images.githubusercontent.com/112146207/231042846-4b76998a-cc03-4c4c-9568-174d36763b7b.png)

---
We will now create 2 Watchlists within Azure Sentinel and ingest geo-data CSV Files from Azure Storage

![image](https://user-images.githubusercontent.com/112146207/231043926-67b9a1bc-b5cf-4b74-81bc-2370a8901560.png)

Now add this information exactly and use YOUR Blob URLs that you copied and saved 

![image](https://user-images.githubusercontent.com/112146207/231044304-b37eee42-396e-4d38-a3b7-24f2d6062360.png)

![image](https://user-images.githubusercontent.com/112146207/231045193-e4b86e10-1d9f-43a7-98da-d447ddcb1955.png)

![image](https://user-images.githubusercontent.com/112146207/231052913-3c103c38-3ee6-4f4c-9efa-8d3832d7252a.png)

For the second watchlist I put this information in

![image](https://user-images.githubusercontent.com/112146207/231053907-bdec4f66-2204-4310-8cd4-0ead229d4c39.png)

Now we have to allow these files to “upload”/load from our storage account into Sentinel/Log Analytics Workspace. The big one will likely take over 24 hours

---
We will go to work analytics workspace and query the watchlists we created just to make sure we can see the records from both watchlists 

It should look something like this

![image](https://user-images.githubusercontent.com/112146207/231058252-685c7062-26c7-427d-970d-34c9fe2d851a.png)

<details close>

---
In this section we will create a Linux VM, and we're going to configure the Windows security event logs from our Windows machine, and the syslog logs from our Linux machine to send to our log analytics workspace. In additional to the VM, we will also configure logging for the NSG (AKA the mini firewalls) and we're going to send flow logs into the log analytics workspace. 

  
To create another Virtual Machine in Azure, use the same Region, Resource Group, and VNet as the previous VM, and name it "linux-vm". 
Avoid choosing B1s for the VM size as it may stop creating logs during a DDoS attack. 
Use a username and password for authentication instead of SSH keys to restrict access. 
Lastly, open up the NSG to all traffic to allow for inbound and outbound traffic control.
  
![image](https://user-images.githubusercontent.com/112146207/231915269-71d8b02a-9a26-4606-80a6-15b9e2dbfb3e.png)
![image](https://user-images.githubusercontent.com/112146207/231915347-62f9b7dc-8417-407e-8c89-868c9030add0.png)


---
 
To enhance the security of your Azure environment, there are three steps that we will take. 
First, enable Microsoft Defender for Cloud for your Log Analytics Workspace. 
 
![image](https://user-images.githubusercontent.com/112146207/231920040-942a930e-8b64-4e57-a570-1c061285996c.png)
   
![image](https://user-images.githubusercontent.com/112146207/231926363-08254f1f-16cb-4063-b121-98b233bd2e3c.png)
 
Make sure everything is checked off and that you have your resource group, subscription, and your log analytics workspace info.
![image](https://user-images.githubusercontent.com/112146207/231927395-28eb3a69-932b-4460-8cad-6641401e2528.png)

We're going to go ahead and enable security policy since we will use it later on

Make sure to click on "security policy" and then click "Add more standards"

![image](https://user-images.githubusercontent.com/112146207/232256225-219fdb5c-777a-452f-bdc3-7b4b71149ba1.png)

We're going to add NIST 800-53: Security and privacy controls and Azure CIS 1.4.0 (latest version)

  ---

We will now enable Defender Plans for both VMs and SQL Instances on VMs to detect and respond to potential security threats
  
Now go back to Microsoft Defender for Cloud and click on "environment settings" and we're going to do the same thing for our work analytics workspace
  
![image](https://user-images.githubusercontent.com/112146207/232257863-6e58db5c-69af-45f5-83cb-f5344f04ce41.png)

![image](https://user-images.githubusercontent.com/112146207/232257971-c3b108d8-5904-4084-ad61-7a53d68eda18.png)

This is from the Windows event log, so we are configuring this to send security events to our log analytics workspace

![image](https://user-images.githubusercontent.com/112146207/232257982-e4af1d6f-14eb-4a93-9ea6-f49c6a99f7aa.png)
  
---
 
Now we will configure logging and log forwarding for our NSG (mini firewall) 
  
We will start by going to our Azure home page, click "windows VM", then go to networking,
click on "windows-vm-nsg" 
  
 

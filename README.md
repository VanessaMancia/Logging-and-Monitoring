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

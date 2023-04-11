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

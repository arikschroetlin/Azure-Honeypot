# Azure-Honeypot
Project to display attack events on a map using Azure Sentinel and a virtual machine as a honeypot.

## Objective
The objective of this project is to familiarize myself with ingesting data into a SIEM and experience live attacks. To do this, I will use a cloud-based virtual machine and gather the information from the events, get the geolocation data from the event data, and create a display for that event data in Azure Sentinel.

## Technologies, Regulations, and Azure Components Employed:

- Virtual Machines
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Azure Network Security Group (NSG)
- Windows Remote Desktop
- PowerShell for Log Automation and API data gathering

## Methodology

- <b>*Creating the honeypot*</b>: I began by deploying a vulnerable virtual machines in Azure, simulating an insecure environment.

- <b>*Exporting event data to ipgeolocation.io API*</b>: Using a continuously running script, I exported all data to the geolocation.io API

- <b>*Monitoring and analysis*</b>: Azure was configured to ingest log source from the geolocation API into a log analytics workspace. I then used Microsoft Sentinel to build a heat map for the frequency of security events based on location.

## Creating the honeypot

Within the virtual machines section of Azure, I selected the create option and provided a name, desired resource group, and the Windows 10 image to use. Under the network settings, I created a Network Security Group (NSG) which was configured with the following settings: ![Inbound Security Rule](https://github.com/arikschroetlin/Azure-Honeypot/assets/110754210/62b1fb3c-eddb-43bb-88ef-ddba8f3d268d)

Once the virtual machine was created, I created a workspace with the Log Analytics Workspace section of Azure. I then selected to have all logs from the new virtual machine ingested into the workspace. The last piece of configuration was logging into the virtual machine and disabling the default Windows Firewall settings:![Windows Firewall](https://github.com/arikschroetlin/Azure-Honeypot/assets/110754210/912eded5-4cc2-46fb-b9e4-394058ee7f78)

## Exporting event data to ipgeolocation.io API

With the configuration done, the security events were coming in but I needed a way to get the geolocation info from the events. To do this, I used a pre-built script and an API key for ipgeolocation.io (https://ipgeolocation.io/), which would send the IP address for each failed login event to the API and would return the desired data. The script would then write that data into a log file stored on the virtual machine. 

## Monitoring and analysis

The final part of this project is to set up a workbook within Azure Sentinel and query the data from the log file stored on the virtual machine. Under Azure Sentinel, I selected the appropriate resource created, selected the workbooks section under threat management, and then used the add workbook tool. From there, I selected the edit button, then add, then add query. The following is the KQL query used to ingest the data into Azure Sentinel:![Sentinel Query](https://github.com/arikschroetlin/Azure-Honeypot/assets/110754210/987ff94f-d625-4041-8e52-005a96cd3887)
With this information finally within Sentinel, we can choose to do what we want with the data. In this case, I chose to display the information on a world map and by assigning the appropriate fields with the geolocation data, I got these results:
![Failed_RDP_Map](https://github.com/arikschroetlin/Azure-Honeypot/assets/110754210/6c6b5174-47b0-4a65-bdc0-773569a66a3d)

## Conclusion

The goal of this project was to familiarize myself with SIEM technology. I accomplished this by using an Azure-hosted virtual machine as a honeypot and by using Azure Sentinel, which was configured to display the attack data onto a world map. As a side lesson from this project, I learned the extent to which exposed systems are targeted by threat actors and how persistent some of them can be, such as the two results I found from Germany and the Netherlands having thousands of attempts to RDP into the honeypot. 

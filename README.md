# Azure Honeypot

## Objective

Get hands on experience connecting Azure Sentinel (SIEM) to an Azure Windows 10 VM acting as a honey pot. Observe live attacks (RDP Brute Force) from all over the world. Use a custom PowerShell script to connect to  IP Geolocation to pinpoint the exact location on Azure Sentinel Map and make a report.

### Skills Learned

- Configuration with Azure Sentinel workbook.
- Configuring Log Analytics Workspace in Azure to ingest custom logs containing geographic location.
- Custom PowerShell script to extract metadata from Windows Event Viewer to be forwarded to third party API to extract 
  geolocation data
- Using third party API like IP Geolocation


### Tools Used

- Microsoft Sentinel (SIEM) 
- PowerShell
- Log Analytics Workspace
- IP Geolocation

## Steps




Step 1- When I first started up my VM honey pot, I was unable to ping my VM. So, I turned off all the firewalls and tried to ping it again, this time being successful. Knowing that is it now accepting ICMP traffic will help in attackers being able to scan my network making it a good target. 

![able to ping my Azure VM with all of my firewalls turned off](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/5c6e126c-3e06-4cb8-9baf-998fe4dc4dcb)

Step 2- I wanted to view the Event Security logs on my VM just to get used to what failed and successful logins look like

![Login to VM successful event ID 4624](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/6910167e-b896-4105-8204-979662fdc43a)
![Failed login on purpose looking for event ID 4625 for failed login attempts](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/9b66f421-5255-4e9e-b2c6-999f625581c3)

Step 3- Ran a PowerShell script to overnight to collect logs for failed RDP attempts and train the extract feature in Log Analytics Workspace. Also used an API key from IP Geolocation to get the coordinates from where the attacks were coming from.
![API ipgeolocation to insert into powershell script](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/db0af003-20de-4908-a718-5d49f6361269)

Step 4- Create a customized a log named "Failed_RDP_with_Geo_CL" in Log Analytics workspace. Let my script run overnight and woke up to the following.
![Let my Azure run overnight and woke up to these failed login attempts ](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/56cd0a42-0f4c-42ee-9e57-b5586a2806a5)

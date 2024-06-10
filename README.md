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




Step 1- When I initially started up my VM honey pot, I encountered an issue where it did not respond to ping requests. To troubleshoot, I disabled all firewalls, after which I was able to successfully ping the VM. This means that the honey pot is now accepting ICMP traffic. While this resolves the initial problem, it also increases the attractiveness of the honey pot as a potential target for attackers, as they can now use it for network reconnaissance 

![able to ping my Azure VM with all of my firewalls turned off](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/5c6e126c-3e06-4cb8-9baf-998fe4dc4dcb)

Step 2- I wanted to review the Event Security logs on my VM to become familiar with the patterns of failed and successful login attempts. By doing so, I aim to develop a better understanding of the typical entries in these logs, which will help me recognize potential security issues or irregularities in the future

![Login to VM successful event ID 4624](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/6910167e-b896-4105-8204-979662fdc43a)
![Failed login on purpose looking for event ID 4625 for failed login attempts](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/9b66f421-5255-4e9e-b2c6-999f625581c3)

Step 3- Ran a PowerShell script to overnight to collect logs for failed RDP attempts and train the extract feature in Log Analytics Workspace. Also used an API key from IP Geolocation to get the coordinates from where the attacks were coming from.
![API ipgeolocation to insert into powershell script](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/db0af003-20de-4908-a718-5d49f6361269)

Step 4- Create a customized a log named "Failed_RDP_with_Geo_CL" in Log Analytics workspace. Let my script run overnight and woke up to the following.
![Let my Azure run overnight and woke up to these failed login attempts ](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/56cd0a42-0f4c-42ee-9e57-b5586a2806a5)

Step 5- Created custom fields running the following script.
![Script to seperat the logs into columns for easier viewing](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/5032fe5f-ff5d-4c57-a6bf-a5ca95fb33e3)

Step 6- Customized the appropriate settings in "Map" settings.
![Final map with all attacks](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/5032cc9a-c8c1-476b-8f51-eb60fbdda4be)


Step 7- Final workbook report
![Final edited map](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/7b54ccc0-f4f9-49f3-9c12-4e51860f645a)


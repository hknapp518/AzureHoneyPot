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

Step 2- I wanted to review the Event Security logs on my VM to become familiar with the patterns of failed and successful login attempts. By doing so, I aim to develop a better understanding of the typical entries in these logs, which will help me recognize potential security issues or irregularities in the future. I used Microsoft Sentinel and KQL to monitor authentication activity on my honeypot virtual machine. I ran queries to detect repeated failed logon attempts from remote IP addresses targeting the VM, filtering for events with ten or more failures in the last five hours to identify potential brute-force attacks. I also queried for successful logon events during the same timeframe to determine if any unauthorized access had occurred.

![Login to VM successful event ID 4624](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/6910167e-b896-4105-8204-979662fdc43a)
![Failed login on purpose looking for event ID 4625 for failed login attempts](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/9b66f421-5255-4e9e-b2c6-999f625581c3)
![Query to see if login attempts were successful](https://github.com/user-attachments/assets/123ec4f6-98fe-4905-ba32-fb3142f4001b)
![Honeypot query failed login attempts](https://github.com/user-attachments/assets/358da08d-f3a7-4e5a-b15b-315871afb915)

Step 3- I ran a PowerShell script overnight to gather logs for failed RDP attempts, aiming to collect valuable data on potential intrusion attempts. I utilized the extract feature in Log Analytics Workspace to efficiently extract and analyze relevant information from these logs. Additionally, I employed an API key from IP Geolocation to retrieve geographic coordinates corresponding to the origin of the attacks, enhancing my understanding of the geographic distribution of potential threats
![API ipgeolocation to insert into powershell script](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/db0af003-20de-4908-a718-5d49f6361269)

Step 4- Created a customized a log named "Failed_RDP_with_Geo_CL" in Log Analytics workspace. Then I let my script run overnight and woke up to the following.
![Let my Azure run overnight and woke up to these failed login attempts ](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/56cd0a42-0f4c-42ee-9e57-b5586a2806a5)

Step 5- Created custom fields running the following script.
![Script to seperat the logs into columns for easier viewing](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/5032fe5f-ff5d-4c57-a6bf-a5ca95fb33e3)

Step 6- Customized the appropriate settings in "Map" settings.
![Final map with all attacks](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/5032cc9a-c8c1-476b-8f51-eb60fbdda4be)


Step 7- Final workbook report
![Final edited map](https://github.com/hknapp518/AzureHoneyPot/assets/125601731/7b54ccc0-f4f9-49f3-9c12-4e51860f645a)

# **Locking Down RDP Access Using NSG, Azure Policy, and Bastion**

To reduce the attack surface of an Azure virtual machine, I updated the associated Network Security Group (NSG) to block RDP access from the public internet. I retrieved my public IP address and created an inbound NSG rule that allows TCP traffic on port 3389 (RDP) only from my specific IP address, using a `/32` mask for exact matching.

I verified that RDP access worked from my machine but was blocked from all other sources. Any existing rules that allowed RDP from `0.0.0.0/0` or `*` were removed or disabled.

To enforce this policy across the environment, I assigned the built-in Azure Policy: “Audit VMs that allow RDP from the Internet.” I also created a custom Azure Policy that denies the creation of any NSG rule which allows inbound RDP from public IP ranges. This policy evaluates any rule targeting port 3389 with a source address of `0.0.0.0/0` or `*`, and blocks it.

As a secure and scalable alternative to exposing RDP, I implemented Azure Bastion. Bastion allows browser-based RDP and SSH access through the Azure portal without exposing the VM to the public internet. This eliminates the need for public IP addresses and open RDP ports, aligning with zero trust architecture principles.

This solution demonstrates how to implement secure access controls, reduce brute-force exposure, and enforce organization-wide standards using native Azure services.




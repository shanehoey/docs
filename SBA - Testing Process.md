---
title: SBA Testing Process
description: 
type:
  - notes
tags:
  - powershell
  - sba
  - directrouting
  - MS Teams
aliases: 
created: 2024-09-06T00:00:00.000Z
modified: 2024-09-06T00:00:00.000Z
gists:
  - id: a124e49ef65e74178825b64a92137fa3
    url: https://gist.github.com/shanehoey/a124e49ef65e74178825b64a92137fa3
    createdAt: 2024-09-11T02:27:21Z
    updatedAt: 2024-09-11T04:33:27Z
    filename: SBA Testing Process.md
    isPublic: false
share: true
---
 
![00-default.png](./assets/00-default.png)

# SBA Testing Process
/enve
The  process assumes the following requirements:

- A Windows PC, used to monitor the SBA and SBC independent to the PC's making telephone calls.
- One or more supported Teams  clients for making and receiving phone calls in survivable mode.
- To simplify testing each user should be logged into a single teams client,
- Users should be registering to the SBA correctly.
- OPTIONS and Inbound pstn calls are Forked correctly, between sbc and sba are correct
- The process to [simulate internet outages](SBA%20-%20Simulated%20Internet%20outage.md) is not covered in this section 

## Detailed Steps

### Step 1 -  Getting ready for testing 

- Connect to the SBA via remote desktop, and clear the cache, database files and syslogs:
	- Stop the Microsoft Teams SBA Service 
	- Delete all files in the c:\program files\microsoft\microsoft sba\cache directory 
	- Delete the file c:\program files\microsoft\microsoft SBA 
	- Delete all files in c:\program files\microsoft\microsoft sba\logs
	- Set the log level to trace in sbasettings.json to trace

	>[!TODO] Add Screenshot

- On the PC that will be used for survivalblity testing 
	- If teams is still running, right-click teams icon and select quit.
	- In a terminal delete all files and folders in the the teams cache folder %userprofile%\appdata\local\Packages\MSTeams_8wekyb3d8bbwe\LocalCache\Microsoft\MSTeams
	- Make sure to log out of all other clients, especially browsers, mobile, and tablet devices.”
	- Disable voice mail for user who is being tested. (to ensure voicemail does not answer call in tests, and during survivability testing 
	- Ping the SBC Fqdn
	- Ping the SBA Fqdn

	>[!TODO] Add Screenshot

- On the PC that will be used to monitor the SBC, SBA
	- Install Syslog software on pc from [AudioCodes Syslog](https://tools.audiocodes.com/install/)
	- Install [Wireshark](https://www.wireshark.org/download.html) 
	- Configure the SBC to send syslogs to pc 

	>[!TODO] Add Screenshot

- Ensure that your teams status is *"Available"*
- Make an outbound PSTN Call 
- Make an inbound PSTN Call 

- on the SBA confirm the 
	- Confirm user is registered on the SBA http://ocalhost:8081/api/v1/diagnostics/internal/registrations
	- http://ocalhost:8081/api/v1/diagnostics/user

 >[!TODO] Add Screenshot




### Step 2 - Simulate Internet outage

- Log out and log back into the teams client.
- Make an outbound PSTN Call 
- Make an outboun
- 
>[!IMPORTANT] Ensure you are marked as available

	>[!TODO] Add Screenshot

- Implement the survivable mode via simulating an internet outage
- 
- Wait for the Teams client to go into survivability 

  

### Step 3 - Make Inbound /Outbound Calls during surviable mode

- Make a outbound PSTN Call, the call should be successful.
- Make a inbound PSTN Call, the call should be successful.
~~If calls in either direction are not successful follow this guide to troubleshoot [SBA - Troubleshooting Teams](SBA%20-%20Troubleshooting%20Teams.md).~~
~~If both calls are successful continue to step 4.~~

### Step 4 - reverting  back online 

- Enable the Internet access again

- Wait for the MS Teams client to exit survivability mode.
- Make a outbound PSTN Call, the call should be successful.
- Make a inbound PSTN Call, the call should be successful.
- Collect all logs
	- Wireshark capture logs (from Teams client and SBA)
	- Teams Client logs (before and after)
	- Syslogs
	- User Registration logs (before and after)
	- SBA Logs
	- Optional  - SBA Config Log
	- Optional - Web Admin Logs
	- Full chain of certificateof both the SBA and SBC.
	- SBASettings.json

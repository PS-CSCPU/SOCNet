# Building a SOC + Honeynet in Azure (Live Traffic)
![Untitled Diagram drawio](https://github.com/user-attachments/assets/f6f1c964-fd8c-4954-80f6-8cc2551e7f86)


## Introduction

For this project, I build a mini honeynet in Microsoft Azure to take log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Public Internet Before](https://github.com/user-attachments/assets/33b450ee-b57e-45b8-9b46-6cb5cb10141d)



## Architecture After Hardening / Security Controls
![Public Internet After](https://github.com/user-attachments/assets/a5144945-2a25-4c95-a16d-63c97f6abc0e)



The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet.

For the "AFTER" metrics, I performed incident response to the security alerts. Then I hardened the Network Security Groups by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint.

## Attack Maps Before Hardening / Security Controls
**[Windows RDP/SMB Auth Failures]**![Windows RDP Before](https://github.com/user-attachments/assets/c552c091-9d06-4282-a0a3-9b7b6686ce6c)


**[Linux Syslog Auth Failures]**![Linux SSH Auth Before](https://github.com/user-attachments/assets/b7089069-da31-4053-8ebd-1c7097d0e258)


**[NSG Allowed Inbound Malicious Flows]**![NSG Malicious Before](https://github.com/user-attachments/assets/3944a4a9-9ef2-4b57-997c-2bd389bb4120)



## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 3/19/2025  9:26:19 AM
Stop Time 3/20/2025  9:26:19 AM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 3358
| Syslog                   | 5548
| SecurityAlert            | 0
| SecurityIncident         | 118
| AzureNetworkAnalytics_CL | 1310

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 3/23/2025  4:29:02 PM
Stop Time	3/24/2025  4:29:02 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 616
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. The incidents created from this lab were resolved using NIST 800-61. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.

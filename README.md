# Building a SOC + Honeynet in Azure (Using Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

For this particular project, I constructed a small-scale honeynet on Azure and integrated log sources from diverse resources into a Log Analytics workspace. This workspace was subsequently utilized by Microsoft Sentinel to develop attack maps, activate alerts, and create incidents. To analyze the effectiveness of the security measures in place, I collected security metrics within an unsecured environment over a 24-hour period. I then proceeded to strengthen the environment with several security controls and collected metrics for another 24-hour period. The results, which I present below, include the following metrics:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The essential elements of the mini SOC Honeynet in Azure include the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

In terms of the "BEFORE" metrics, all resources were initially set up and made available on the internet. The Virtual Machines were configured with Network Security Groups and built-in firewalls that were completely open to all traffic, while all other resources were deployed with public endpoints that were easily accessible from the internet. Private Endpoints were not needed due to the nature of the deployment.

Concerning the "AFTER" metrics, the Network Security Groups underwent rigorous hardening procedures by allowing ONLY authorized traffic to pass through, specifically from my admin workstation. Furthermore, all other resources were fortified by their built-in firewalls and additional security measures were put in place, including Private Endpoints that ensured secure access.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/XebwxCg.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/CEYb2yY.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/8Oe6kUP.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics I measured in the insecure environment for 24 hours:
Start Time 2023-05-06 15:35 CST
Stop Time 2023-05-07 15:35 CST

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 61078
| Syslog                   | 3169
| SecurityAlert            | 6
| SecurityIncident         | 294
| AzureNetworkAnalytics_CL | 1026

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics I measured in the environment for another 24 hours, but after security controls were applied:
Start Time 2023-05-07 10:28
Stop Time	2023-05-08 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10963
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Percentage of change in Metrics After Hardening / Security Controls

The following table shows the metrics I measured in the environment for another 24 hours, but after security controls were applied:
Start Time 2023-05-07 10:28
Stop Time	2023-05-08 15:37

| Metric                   | % of change after securing environment
| ------------------------ | -----
| SecurityEvent            | 82.05% Less Security Events
| Syslog                   | 99.21% Less Unauthorized System logs
| SecurityAlert            | 100% Less Security Alerts
| SecurityIncident         | 100% Less Security Incidents
| AzureNetworkAnalytics_CL | 100% Less Network Security Group Inbound Malicious flows allowed

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastially reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.

# Building a SOC + Honeynet in Azure (Live Traffic)
<img width="960" height="720" alt="image" src="https://github.com/user-attachments/assets/d515126b-1f1e-493d-81be-efd8ecd40efd" />


## Introduction

In this project, I deployed a mini honeynet in Microsoft Azure and configured multiple log sources from various resources to feed into a centralized Log Analytics workspace. This workspace was integrated with Microsoft Sentinel to create attack maps, generate alerts, and manage incidents. I collected baseline security metrics from the unsecured environment over a 24-hour period, implemented a series of security controls to harden the environment, and then measured the metrics again over the following 24 hours. The results below reflect a comparison between the insecure and secured states, focusing on the following metrics:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening and Enabling Security Controls
<img width="960" height="720" alt="image" src="https://github.com/user-attachments/assets/04cf76ff-d1dd-4aca-91b1-b68fa0703089" />


## Architecture After Hardening and Enabling Security Controls
<img width="960" height="720" alt="image" src="https://github.com/user-attachments/assets/17bab685-5a0b-40e1-a706-9c7d90661360" />


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

**BEFORE Metrics:**  
All resources were initially deployed with unrestricted internet exposure. Virtual Machines had fully open Network Security Groups (NSGs) and built-in firewalls, allowing all inbound and outbound traffic. Additionally, all other resources were configured with public endpoints accessible from the internet, with no implementation of Private Endpoints.  

**AFTER Metrics:**  
Network Security Groups were hardened to block all inbound traffic except from my designated administrative workstation. All other resources were secured using both their built-in firewalls and Azure Private Endpoints, effectively removing public internet exposure.


## Attack Maps Before Hardening / Security Controls
<img width="1494" height="830" alt="image" src="https://github.com/user-attachments/assets/84ebee27-22cc-4a47-8fd6-00af33449eee" />
<br>
<img width="1500" height="752" alt="image" src="https://github.com/user-attachments/assets/fc5c3e37-7ca7-4ce0-9161-4025acd93ae0" />
<br>
<img width="2208" height="1238" alt="image" src="https://github.com/user-attachments/assets/f0c84419-aec7-4c04-9d7e-d25de6e5a8ca" />
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2025-08-09 00:06:09
Stop Time 2025-08-10 00:04:29

| Metric                   | Count          |
| ------------------------ | -------------- |
| SecurityEvent            | 15348          |
| Syslog                   | 2765           |
| SecurityAlert            | 18             |
| SecurityIncident         | 491            |
| AzureNetworkAnalytics_CL | 1027           |


## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2025-08-10 06:17
Stop Time	2025-08-11 09:51

| Metric                   | Count         | % Change   |
| ------------------------ | ------------- | ---------- |
| SecurityEvent            | 6900          | -55.05%    |
| Syslog                   | 22            | -99.20%    |
| SecurityAlert            | 0             | -100.00%   |
| SecurityIncident         | 0             | -100.00%   |
| AzureNetworkAnalytics_CL | 0             | -100.00%   |

## Conclusion

In this project, I deployed a mini honeynet in Microsoft Azure and integrated log sources from multiple resources into a centralized Log Analytics workspace. Microsoft Sentinel was then used to generate alerts and create incidents based on the ingested data. Security metrics were collected in the insecure environment before implementing controls, and again after applying hardening measures. The results showed a significant reduction in security events and incidents, clearly demonstrating the effectiveness of the applied controls.

It is important to note that if the network resources had been actively utilized by regular users, the 24-hour post-implementation window may have produced a higher volume of security events and alerts.

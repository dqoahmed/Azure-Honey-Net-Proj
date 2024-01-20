# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction
 In this project, my objective is to establish a cloud honeynet within Microsoft Azure, encompassing the creation of essential resources such as virtual machines, key vaults, and storage. Subsequently, I will configure Azure Active Directory, implement log analytics, and integrate a Security Information and Event Management (SIEM) system. Notably, the intentional configuration of vulnerability within the virtual machines and other resources will be undertaken to simulate external attacks.

The project involves the creation of user accounts with specific permissions, linking all resources to a centralized log repository within the log analytics workspace, facilitating log ingestion. This integrated system will then be connected to the SIEM to enable the triggering of alerts, generation of attack maps, and observation of incidents.

Following this phase, the environment will undergo a reconfiguration process, incorporating security controls and system hardening measures. The ultimate aim is to conduct a comparative analysis between the secure and insecure states of the environment, thereby enhancing its overall resilience and security posture.

## Honey-Net evnvironment components
The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

## Virtual Machine and SSMS set up.

  Here I created two virtual machines and added them in the same resource group and configured the network resource group for both VMs to be vulnerable by modifying the inbound security rules on the network security group and turned off   the VMs firewall defender as part of the honey net. Also, I installed SQL Server in Windows VM as another end point for people to attack. I exposed our network with the intent to observe our logs and secure them later. Then I added     another attack VM with separate network, location and resource group and installed SQL to attack the other VMs in order to generate and observe logs and then secure the environment for comparison.
  
  ![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/4fe3279f-73ef-429b-8304-03a723ca4e41)
  ![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/7b4120de-9bf2-4abb-b85b-c240333937f9)
  
## Storgae Account
   Created a storage account within the same resource group as the other resources.
  ![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/ee92ad75-efa5-4783-ae1a-d3ed116af617)
  
## Key Vault
  ![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/96b4d465-b5b0-412a-a261-b18647d53af7)


## Azure Active Directory (AAD)

  AAD is Cloud based identity and Access management that manages user accounts for people in the organization, stores user accounts, and can manage access to other cloud resources. Users can be granted different levels of permission       such  Tenant level, management level, Subscription level, and resource groups level and be  assigned a role or multiple roles within that level.
  I created users and assigned roles such as tenant level global reader, subscription level reader and contributor.
   ![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/0dcf8857-4e29-45a8-9a3f-3df5802ca4f2)
   
 Setting up logs for Azure Active Directory will help us forward logs to the Log Analytics Workspace. After enabling audit and sign-in log for AAD, I’ll create a user, assign roles and play around with it to show audit log in Log 
 Analytics Workspace
   ![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/2fef573b-1459-43d2-a6f2-3c8114a74c7b)


## Creating LOG Analytics workspace 

  Setting up log analytics workspace which is a central log repository or log container for all applications and infrastructure which helps you collect, monitor, analyze, and query data.
  ![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/f2f457b7-f45e-4e23-9c58-f56ecba0ee00)

## Creating Microsoft Sentinel SIEM 
  Azure Sentinel is a Microsoft SIEM, that aids in detecting, investigating, and responding to security threats across enterprises. With advanced data collection, detection, seamless integration, and robust incident response tools.
  Now creating Microsoft Sentinel and connecting it to log analytics workspace. 
    ![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/6b72741b-87f4-4ba3-9b82-0e782f63679a)
 

## Microsoft defender
   Enabling Microsoft defender for cloud which allows us to collect all event logs from vms, subscriptions to forward them to the central repository log analytics workspace.
   ![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/5cf2b470-8d8c-4759-8f4c-752b39a3e63f)

 
## 	Enabling log collection for VMs and network security groups
  Enabling flow logs for both Linux and Windows network security groups to forward log data to the central log workspace.
 ![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/58a11530-3d6b-4443-a669-13b55383a92a)
 ![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/0f219196-28b9-42dc-84c5-7926bb3c9610)
 
 ## Setting up logs for Azure Active Directory
  Setting up logs for Azure Active Directory will help us forward logs to the Log Analytics Workspace. After enabling audit and sign-in log for AAD, I’ll create a user, assign roles and play around with it to show audit log in Log    a analytics Workspace.
  ![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/4cec27d0-9447-4c4b-800c-fca4d822cc63)

 
 ## 	Enabling Loging and monitoring at the subscription level for the activity log
  Activating Loging and monitoring in the Azure monitor to export event activities that happen in the resources such as creating, deleting, and changing and who performed to log analytics workspace. I’ll create and delete resource 
  groups  to generate logs in the log analytics workspace. 
 ![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/f4eb06ab-3dbb-4d09-b36c-4ba2d5df5d78)


## Enabling Loging and monitoring at the Resource level for storage and key vault
Activating log collection for storage and key vault through diagnostics as part of our resources. I’ll upload a file to a container in the storage and edit then create secret passwords and view the passwords in the key vault to     generate logs in the log analytics.
![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/c74e43f1-5ad3-43ee-9392-f8220a6fdaf5)
![image](https://github.com/dqoahmed/Azure-Honey-Net-Proj/assets/156861134/1b2c84f6-3c5f-48c4-9bd8-ecc7d6aaba26)



 

 

 
 




















For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.

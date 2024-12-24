DigiAsset Core Installation Guide on Windows 10/11 with Hyper-V
This guide will help you install and configure DigiAsset Core on a Windows 10/11 machine using Hyper-V.

Prerequisites
System Requirements:

Windows 10 or 11 with Hyper-V support.
At least 150GB of free disk space.
Administrative privileges.
Credentials:

Username: digiasset
Password: tothemoon
Downloads:

DigiAsset Core
PuTTY (SSH Client)
Step 1: Preparing the DigiAsset Files
Download the DigiAsset.zip file from the repository.
Extract the contents to C:\digiasset.
Note: Ensure the files are in the root directory C:\ as commands depend on this location.

Delete the digiasset.zip file after extraction to reclaim disk space.
Step 2: Enable Hyper-V on Windows
Open a PowerShell terminal as Administrator.
Run the following command to enable Hyper-V:
powershell
Copy code
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
Reboot your system.
Step 3: Configure Hyper-V
Open PowerShell as Administrator and create a virtual switch:
powershell
Copy code
New-VMSwitch -Name "Lan" -NetAdapterName Ethernet -AllowManagementOS:$true
Import the virtual machine:
powershell
Copy code
Import-VM -Path "C:\DigiAsset\Virtual Machines\A4F1C73E-3384-4799-8E42-2A42A4FCB6B9.vmcx"
Connect the network adapter to the virtual switch:
powershell
Copy code
Get-VM "DigiAsset" | Get-VMNetworkAdapter | Connect-VMNetworkAdapter -SwitchName "Lan"
Start the virtual machine:
powershell
Copy code
Start-VM -Name DigiAsset
Step 4: Launch and Configure DigiAsset Core
Open Hyper-V Manager and connect to the DigiAsset VM.
Run the following command to check the IP address of the VM:
bash
Copy code
showipaddr
Step 5: SSH into the VM
Install and launch PuTTY.
Use the IP address from the previous step to connect to the VM via SSH.
Login with:
Username: digiasset
Password: tothemoon
Step 6: Run DigiAsset Core
Navigate to the DigiAsset Core directory:
bash
Copy code
cd /home/digiasset/DigiAsset_Core/bin
Launch DigiAsset Core:
bash
Copy code
./digiasset_core
Step 7: Configuration Wizard
Follow the prompts in the configuration wizard:

DigiByte Core:
Running locally: Y
Port: 14022
Username: user
Password: pass11
IPFS:
Running locally: Y
DigiByte Address:
Example: D6oEbszUbYBjiY6BeVW6g8cSTmkY7TahXA
Pruning Mode:
Recommended: Y
Database Bootstrap:
From IPFS: Y
Allow All RPC Commands:
Recommended: Y
Step 8: Configure Firewall
Open the following ports on your external firewall and point them to the IP address of the Ubuntu VM:

TCP/UDP 4001
TCP 12024
You’re all set! DigiAsset Core should now be running on your system. If you encounter any issues, refer to the DigiAsset Core GitHub repository for further assistance.

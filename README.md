# DigiAsset Core Installation Guide on Windows 10/11 with Hyper-V

This guide will help you install and configure DigiAsset Core on a Windows 10/11 machine using Hyper-V. This is a pre-configured installation that will require minimal setup and install/configure time. This is perfect if you are a Windows user and have an extra machine sitting around and would like to participate.

## Prerequisites

1. **System Requirements**:
   - Windows 10 or 11 with Hyper-V support.
   - At least 150GB of free disk space.
   - Administrative privileges.

2. **Credentials for the VM**:
   - Username: `digiasset`
   - Password: `tothemoon`

3. **Downloads**:
   - [DigiAsset Core](https://github.com/DigiAsset-Core/DigiAsset_Core)
   - [PuTTY (SSH Client)](https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.82-installer.msi)

---

## Step 1: Preparing the DigiAsset Files

1. Download the `DigiAsset.zip` file from the folowing Dropbox Link - (https://www.dropbox.com/scl/fi/c3yzrag5eheb363gqsmbb/DigiAsset.zip?rlkey=becbvqya415lhuihl8efyayvb&st=iyfauste&dl=0)
2. Extract the contents to `C:\digiasset`.
   > **Note**: Ensure the files are in the root directory `C:\` as commands depend on this location.
3. Delete the `digiasset.zip` file after extraction to reclaim disk space.

---

## Step 2: Enable Hyper-V on Windows

1. Open a PowerShell terminal as Administrator.
2. Run the following command to enable Hyper-V:
   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
   ```
3. Reboot your system.

---

## Step 3: Configure Hyper-V

** Note this assumses the network card name is "Ethernet"
1. Open PowerShell as Administrator and create a virtual switch:
   ```powershell
   New-VMSwitch -Name "Lan" -NetAdapterName Ethernet -AllowManagementOS:$true
   ```
2. Import the virtual machine:
   ```powershell
   Import-VM -Path "C:\DigiAsset\Virtual Machines\A4F1C73E-3384-4799-8E42-2A42A4FCB6B9.vmcx"
   ```
3. Connect the network adapter to the virtual switch:
   ```powershell
   Get-VM "DigiAsset" | Get-VMNetworkAdapter | Connect-VMNetworkAdapter -SwitchName "Lan"
   ```
4. Start the virtual machine:
   ```powershell
   Start-VM -Name DigiAsset
   ```

---

## Step 4: Launch and Configure DigiAsset Core

1. Open **Hyper-V Manager** and connect to the `DigiAsset` VM.
2. Run the following command to check the IP address of the VM:
   ```bash
   showipaddr
   ```
3. Note the internal and external IP Addresses of your system.

---

## Step 5: SSH into the VM

1. Install and launch [PuTTY](https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.82-installer.msi).
2. Use the IP address from the previous step to connect to the VM via SSH.
3. Login with:
   - Username: `digiasset`
   - Password: `tothemoon`

---

## Step 6: Run DigiAsset Core

1. Navigate to the DigiAsset Core directory:
   ```bash
   cd /home/digiasset/DigiAsset_Core/bin
   ```
2. Launch DigiAsset Core:
   ```bash
   ./digiasset_core
   ```

---

## Step 7: Configuration Wizard

Follow the prompts in the configuration wizard:

1. **DigiByte Core**:
   - Running locally: `Y`
   - Port: `14022`
   - Username: `user`
   - Password: `pass11`
2. **IPFS**:
   - Running locally: `Y`
3. **DigiByte Address**:
   - Example: `D6oEbszUbYBjiY6BeVW6g8cSTmkY7TahXA`
4. **Pruning Mode**:
   - Recommended: `Y`
5. **Database Bootstrap**:
   - From IPFS: `Y`
6. **Allow All RPC Commands**:
   - Recommended: `Y`

---

## Step 8: Configure Firewall

Open the following ports on your external firewall and point them to the IP address of the Ubuntu VM:

- **TCP/UDP 4001**
- **TCP 12024**

---

## Step 9: Troubleshooting - How do I know it's working?

You can SSH into the server using the local IP address and  run the following 2 commands

- tail /home/digiasset/.digibyte/debug.log - This will show the activity from the DigiByte Wallet
- tail /home/digiasset/DigiAsset_Core/bin/debug.log - This will show the activity from the DigiAsset Core Applcation 

You’re all set! DigiAsset Core should now be running on your system. If you encounter any issues, refer to the [DigiAsset Core GitHub repository](https://github.com/DigiAsset-Core/DigiAsset_Core) for further assistance.

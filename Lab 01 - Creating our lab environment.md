# Lab 1: Configuring our lab environment

The main assessment for this module is a large practical test during exam week. The assessment will be a simulated penetration test on a small company network, for both their wired and wireless networks. To complete the penetration test you'll need your own laptop, hardware, backups etc. and any tools or resources you might need for the day. Having a fully working attack machine will be essential. 

All of the lab activities and in-class demos will be completed using the setup outlined below, however you are free to use any setup and tools you prefer. 
> :warning: I won't be troubleshooting any issues on different setups, outside of the suggested lab environment. 

___

## Recommended Lab Setup:

1. VirtualBox hypervisor 
2. Kali Linux VM  
-- recommended, but any modern linux VM should be fine
___

Download:
VirtualBox: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads) (Download the package for your own OS) 
Kali Linux image (latest version): [https://www.kali.org/get-kali/](https://www.kali.org/get-kali/) (For most students, I'm recommending the pre-built virtual box image) 


## Setup Instructions 


1. Install VirtualBox - Run the downloaded installer and follow the on-screen instructions.
   
2. Extract the Kali VirtualBox image. The Downloaded image file is a 7zip compressed archive, so you'll first need to extract the VirtualBox image before importing to Virtualbox.
   
   -- On windows you can download the offical 7zip app to extract the image. [https://www.7-zip.org/](https://www.7-zip.org/)
   -- On Linux you can type ```7z x filename.7z```
   -- On Mac the default archive tool should be able to extract the image.

3. Start VirtualBox and select ‘Add’. We then navigate to the location our VM is downloaded and find the .vbox file.
4. You can change any of the settings to suit your own system, and then start the VM.
5. The default login is kali for the user and kali for the password!

> 💡 Base Memory: Allocate a minimum of 2048 MB RAM (more for better performance).
> 💡 Processors: 1 CPU (more for better performance)


6. 
7. Create a new VM: Open VirtualBox and click “New.” Choose “Expert” mode for more control.





1. [Download and install Oracle VirtualBox](https://www.virtualbox.org/manual/UserManual.html#intro-installing)
2. [Download and install Kali Linux](https://www.kali.org/docs/virtualization/install-virtualbox-guest-vm/)
3. [Tweaking our Initial Kali setup](3_tweaking_kali.md)
4. [Getting Started with Github](4_introducing_github.md)
5. [Installing, Configuring and using VSCode](5_setup_vscode.md)


## Older instructions:
1. [Getting a VMWare Workstation Pro Licence](1_vmware_licence.md)
2. [Downloading and using Kali Linux](2_kali_linux.md)
3. [Tweaking our Initial Kali setup](3_tweaking_kali.md)
4. [Getting Started with Github](4_introducing_github.md)
5. [Installing, Configuring and using VSCode](5_setup_vscode.md)



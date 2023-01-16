# Windows Fundamentals!
## @Author : M3tr1c_r00t
### HTB_Academy!
### Windows Versions...

![image](https://user-images.githubusercontent.com/99975622/212542317-d8ed1c5e-3167-4cd5-bd6c-a77494a66e44.png)

We can use the <a href="https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-wmiobject?view=powershell-5.1">get-wmiobject</a> in powershell to get more information on a system.

Also, we can use the <a href="https://docs.microsoft.com/en-us/powershell/scripting/developer/cmdlet/cmdlet-overview?view=powershell-7"> cmd-outlet</a>

Checking the Version and Build Number...
```
 Get-WmiObject -Class win32_OperatingSystem| select Version,BuildNumber
```
![image](https://user-images.githubusercontent.com/99975622/212542657-a4fac87c-02c9-461b-8976-82226ce3606b.png)
Checking the Bios information...
```
Get-WmiObject -Class win32_Bios
```
![image](https://user-images.githubusercontent.com/99975622/212542793-49bb8fa6-491d-4142-8965-4cbe12c49951.png)

### Accessing Windows
#### Local Access
This refers to accessing a computer physically.
#### Remote Access
This refers to accessing a computer over a network.
Some of the most common remote access technologies include:
- Virtual Private Networks (VPN)
- Secure Shell (SSH)
- File Transfer Protocol (FTP)
- Virtual Network Computing (VNC)
- Windows Remote Management (or PowerShell Remoting) (WinRM)
- Remote Desktop Protocol (RDP)

### Remote Desktop Protocol (RDP)
RDP uses a client/server architecture where a client-side application is used to specify a computer's target IP address or hostname over a network where RDP access is enabled.

The target computer where RDP remote access is enabled is considered the server. 

RDP listens by default on logical port 3389.

#### Connection to a windows target from a windows host...
 we can use the built-in RDP client application called Remote Desktop Connection (mstsc.exe).
 ![UsingRemoteDesktopConnection](https://user-images.githubusercontent.com/99975622/212544854-6a801d78-5d94-40fa-8bab-ff55b2111970.gif)
 
 Saving The RDP profiles for easy login/connection
 ![SavingRDPConnections](https://user-images.githubusercontent.com/99975622/212544952-b26dfe0e-c437-4e5a-9b39-439834de1abc.gif)
 
 #### Connection to a windows target from a Linux host...
 For linux, we can connect using xfreerdp
 ```
 xfreerdp /v:<targetIp> /u:htb-student /p:Password
 ```
 ![ConnectingwithXfreerdp](https://user-images.githubusercontent.com/99975622/212545110-52d36dda-8072-463e-89fd-10dd2c5ffc68.gif)
 
 Other RDP clients exist, such as Remmina and rdesktop.
 
 ### Operating System Structure
 The root directory is usually in the c drive.
 Some of the directories in the root system include:
![image](https://user-images.githubusercontent.com/99975622/212546385-64d1728a-ba5f-4794-a111-0221ae8a5483.png)
To list Directories in a tree like manner;
```
tree "c:\Program Files (x86)\VMware"
```
![image](https://user-images.githubusercontent.com/99975622/212546554-eb5fa48e-1d54-4ba1-864e-53ea05d815a5.png)

You can use this command to control the amount of output coming from the terminal...
```
tree C:\ /f | more
```

### File System
There are 5 types of Windows file systems: FAT12, FAT16, FAT32, NTFS, and exFAT.
FAT12 and FAT16 are no longer used on modern Windows operating systems. 

#### FAT32
FAT32 (File Allocation Table) is widely used across many types of storage devices such as USB memory sticks and SD cards but can also be used to format hard drives. The "32" in the name refers to the fact that FAT32 uses 32 bits of data for identifying data clusters on a storage device.

Pros of FAT32:
- Device compatibility - it can be used on computers, digital cameras, gaming consoles, smartphones, tablets, and more.
- Operating system cross-compatibility - It works on all Windows operating systems starting from Windows 95 and is also supported by MacOS and Linux.

Cons of FAT32:
- Can only be used with files that are less than 4GB.
- No built-in data protection or file compression features.
- Must use third-party tools for file encryption.

#### NTFS
NTFS (New Technology File System) is the default Windows file system since Windows NT 3.1. In addition to making up for the shortcomings of FAT32, NTFS also has better support for metadata and better performance due to improved data structuring.

Pros of NTFS:
- NTFS is reliable and can restore the consistency of the file system in the event of a system failure or power loss.
- Provides security by allowing us to set granular permissions on both files and folders.
Supports very large-sized partitions.
- Has journaling built-in, meaning that file modifications (addition, modification, deletion) are logged.

Cons of NTFS:
- Most mobile devices do not support NTFS natively.
- Older media devices such as TVs and digital cameras do not offer support for NTFS storage devices.

#### Permissions
![image](https://user-images.githubusercontent.com/99975622/212547237-f3690da2-1a0b-4f5e-86c9-a42c83f77e8a.png)

Files and folders inherit the NTFS permissions of their parent folder for ease of administration, so administrators do not need to explicitly set permissions for each file and folder.
#### Integrity Control Access Control List (icacls)
We can list out the NTFS permissions on a specific directory by running either icacls from within the working directory or icacls C:\Windows against a directory not currently in.

![image](https://user-images.githubusercontent.com/99975622/212548116-d38fe2d8-49c1-40b9-8b60-13b27fbeda2f.png)

The resource access level is list after each user in the output. The possible inheritance settings are:
- (CI): container inherit
- (OI): object inherit
- (IO): inherit only
- (NP): do not propagate inherit
- (I): permission inherited from parent container

Basic access permissions are as follows:
- F : full access
- D :  delete access
- N :  no access
- M :  modify access
- RX :  read and execute access
- R :  read-only access
W :  write-only access

We can add and remove permissions via the command line using icacls. 
The command icacls c:\users /grant joe:f we can grant the joe user full control over the directory.
But given that (oi) and (ci) were not included in the command, the joe user will only have rights over the c:\users folder but not over the user subdirectories and files contained within them.

These permissions can be revoked using the command icacls c:\users /remove joe.

<a href="https://ss64.com/nt/icacls.html" > icacls commands lists</a>

### NTFS vs. Share Permissions
The Server Message Block protocol (SMB) is used in Windows to connect shared resources like files and printers.
Share permissions include:
![image](https://user-images.githubusercontent.com/99975622/212549081-30ebe800-07e8-49ad-9049-93b4a0396f5b.png)
NTFS Basic permissions
![image](https://user-images.githubusercontent.com/99975622/212549109-2435b510-0c03-43c3-9646-717ce974783a.png)
NTFS special permissions
![image](https://user-images.githubusercontent.com/99975622/212549165-999d8a30-3962-49fe-8c44-fee8976a662b.png)
Keep in mind that NTFS permissions apply to the system where the folder and files are hosted. Folders created in NTFS inherit permissions from parent folders by default. It is possible to disable inheritance to set custom permissions on parent and subfolders, as we will do later in this module. The share permissions apply when the folder is being accessed through SMB, typically from a different system over the network. This means someone logged in locally to the machine or via RDP can access the shared folder and files by simply navigating to the location on the file system and only need to consider NTFS permissions. The permissions at the NTFS level provide administrators much more granular control over what users can do within a folder or file.

### Creating a Network Share
- First off, create an empty folder...
( Most large enterprise environments, shares are created on a Storage Area Network (SAN), Network Attached Storage device (NAS), or a separate partition on drives accessed via a server operating system like Windows Server. If we ever come across shares on a desktop operating system, it will either be a small business or it could be a beachhead system used by a penetration tester or malicious attacker to gather and exfiltrate data.)

- Making the Folder a Share...
(Go to the folder properties, in the sharing tab, click on advanced sharing. Click on share this folder. Note you can limit the number of users to access the share at once. Note that  there is an access control list (ACL) for shared resources. The ACL contains access control entries (ACEs) which are made up of users and groups. You can set the permissions for everyone , either read,write or both.)
![image](https://user-images.githubusercontent.com/99975622/212549670-b5294044-852d-4da5-8a8e-436df10eb1f4.png)
Setting permissions
![image](https://user-images.githubusercontent.com/99975622/212549696-7b99c9a9-c1e0-46a3-8b43-482879df6797.png)
And done!

In our case,we are being blocked if we try to connect through our linux system.
It is also important to note that when a Windows system is part of a workgroup, all netlogon requests are authenticated against that particular Windows system's SAM database. When a Windows system is joined to a Windows Domain environment, all netlogon requests are authenticated against Active Directory.

The primary difference between a workgroup and a Windows Domain in terms of authentication, is with a workgroup the local SAM database is used and in a Windows Domain a centralized network-based database (Active Directory) is used. We must know this information when attempting to logon & authenticate with a Windows system. Consider where the htb-student account is hosted to properly connect to the target.

In terms of the firewall blocking connections, this can be tested by completely deactivating each firewall profile in Windows or by enabling specific predefined inbound firewall rules in the Windows Defender Firewall advanced security settings. Like most firewalls, Windows Defender Firewall permits or denies traffic (access & connection requests in this case) flowing inbound &/or outbound

The different inbound and outbound rules are associated with the different firewall profiles in defender.

Windows Defender Firewall Profiles:

- Public
- Private
- Domain

It is a best practice to enable predefined rules or add custom exceptions rather than deactivating the firewall altogether. 

#### Creating a Mounting Point...
 Once a connection is established with a share, we can create a mount point from our Pwnbox to the Windows 10 target box's file system.
 
 We can create a mounting point if the files that are in the smb shares are super large.
 In this case, we need to consider the NTFS permissions.
 ![image](https://user-images.githubusercontent.com/99975622/212554231-4d9bc915-176c-45d1-a10a-b8e20fada3ba.png)
 
 Mounting command...
 ```
 sudo mount -t cifs -o username=htb-student,password=Academy_WinFun! //<TARGET_IP>/"Share_Name" /home/user/Desktop/
 ```
 If the command doesnt work, we need to install cifs-utils
 ```
 sudo apt-get install cifs-utils
 ```
 
#### Monitoring Shares
We can use the command 'net share' to see all the available shares in the system.
 
Computer Management is another tool we can use to identify and monitor shared resources on a Windows system.

We can poke around in Shares, Sessions, and Open Files to get an idea of what information this provides us. Should there be a situation where we assist an individual or organization with responding to a breach related to SMB, these are some great places to check and start to understand how the breach may have happened and what may have been left behind.

#### Viewing Share access logs in Event Viewer
Event Viewer is another good place to investigate actions completed on Windows. Almost every operating system has a logging mechanism and a utility to view the logs that were captured. Know that a log is like a journal entry for a computer, where the computer writes down all the actions that were performed and numerous details associated with that action. We can view the logs created for every action we performed when accessing the Windows 10 target box, as well as when creating, editing and accessing the shared folder.

![image](https://user-images.githubusercontent.com/99975622/212555677-b5bf2cbe-cc4b-40bb-9e7c-f21b09160c00.png)

### Windows Services & Processes
#### Windows Services
Windows services are managed via the Service Control Manager (SCM) system, accessible via the services.msc MMC add-in.
This add-in provides a GUI interface for interacting with and managing services and displays information about each installed service. This information includes the service Name, Description, Status, Startup Type, and the user that the service runs under.

It is also possible to query and manage services via the command line using sc.exe using __PowerShell__ cmdlets such as **__Get-Service__**.

This shows the first 2 running services...
```
Get-Service | ? {$_.Status -eq "Running"} | select -First 2 |fl
```

Service statuses can appear as __Running, Stopped, or Paused,__ and they can be set to start manually, automatically, or on a delay at system boot. 
Services can also be shown in the state of __Starting or Stopping__ if some action has triggered them to either start or stop. 

Windows has three categories of services: __Local Services, Network Services, and System Services.__
 Services can usually only be created, modified, and deleted by users with administrative privileges.
 
 In Windows, we have some critical system services that cannot be stopped and restarted without a system restart. If we update any file or resource in use by one of these services, we must restart the system.
 
 ![critical_system_services](https://user-images.githubusercontent.com/99975622/212779910-428b2217-8e01-4fcd-a6b2-1879b1d566b9.png)
  
 We can find info on other services here:
 ```
 https://en.wikipedia.org/wiki/List_of_Microsoft_Windows_components#Services
 ```
 
#### Processes
Processes associated with installed applications can often be terminated without causing a severe impact on the operating system. Certain processes are critical and, if terminated, will stop certain components of the operating system from running properly. Some examples include the Windows Logon Application, System, System Idle Process, Windows Start-Up Application, Client Server Runtime, Windows Session Manager, Service Host, and Local Security Authority Subsystem Service (LSASS) process.

#### Local Security Authority Subsystem Service (LSASS)
__lsass.exe__ is the process that is responsible for enforcing the security policy on Windows systems.
When a user attempts to log on to the system, this process verifies their log on attempt and creates access tokens based on the user's permission levels.
#### Sysinternals Tools
This is a set of portable Windows applications that can be used to administer Windows systems (for the most part without requiring installation). 
The suite includes tools such as Process Explorer, an enhanced version of Task Manager, and Process Monitor, which can be used to monitor file system, registry, and network activity related to any process running on the system. Some additional tools are TCPView, which is used to monitor internet activity, and PSExec, which can be used to manage/connect to systems via the SMB protocol remotely.
These tools can be useful for penetration testers to, for example, discover interesting processes and possible privilege escalation paths as well as for lateral movement.

#### Process Explorer
It is a part of the Sysinternals tool suite.
This tool can show which handles and DLL processes are loaded when a program runs.

Process Explorer shows a list of currently running processes, and from there, we can see what handles the process has selected in one view or the DLLs and memory-swapped files that have been loaded in another view.

The tool can also be used to analyze parent-child process relationships to see what child processes are spawned by an application and help troubleshoot any issues such as orphaned processed that can be left behind when a process is terminated.




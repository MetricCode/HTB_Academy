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



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

Check proccess listing...
```
 Get-WmiObject -Class Win32_Process
```

Check on services listing ...
```
 Get-WmiObject -Class Win32_Service

```

To check on powershell commands, we can use this site...
```
https://ss64.com/ps/
```


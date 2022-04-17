# Windows cli

### Update Service

```
C:\> sc config "TIBHawkAgent-RCEAIABC-ASMET359" binPath= "D:/app/tibco/tra/domain/RCEAIABC/hawkagent_RCEAIABC.exe"
[SC] ChangeServiceConfig SUCCESS
```

### Clear DNS Cache

```
ipconfig /flushdns
```

### Clear Windows Saved Passwords

```
rundll32.exe keymgr.dll, KRShowKeyMgr
```

### Copy out to clipboard

````
dir | clip
​```When you do not want to do a ``svn export``

````

### Remove SVN folders in windows

```
FOR /F "tokens=*" %G IN ('DIR /B /AD /S *.svn*') DO RMDIR /S /Q "%G"
```

### Disable group policy

To Disable Group Policy:

```
REG add "HKCU\Software\Policies\Microsoft\MMC\{8FC0B734-A0E1-11D1-A7D3-0000F87571E3}" /v Restrict_Run /t REG_DWORD /d 1 /f
```

To Enable Group Policy:

```
REG add "HKCU\Software\Policies\Microsoft\MMC\{8FC0B734-A0E1-11D1-A7D3-0000F87571E3}" /v Restrict_Run /t REG_DWORD /d 0 /f
```

### Install Telnet Client

```
pkgmgr /iu:"TelnetClient"
```

### Remote start windows service

```
sc \\machine (start|stop|query) <service>
```

```
sc \\fuj51940 start albd
```

### Logged in Users

#### To list

```
qwinsta /server:<server>
```

```
 SESSIONNAME       USERNAME                 ID  STATE   TYPE        DEVICE
                                             0  Disc    rdpwd
 rdp-tcp                                 65536  Listen  rdpwd
 console                                     5  Conn    wdcon
                   user1                    1  Disc    rdpwd
 rdp-tcp#38        user2                   2  Active  rdpwd
```

#### To kill

```
logoff /server:<server> <session id>
logoff /server:<server> 2
```

```
RWinsta /Server:<server>
```

### Add local users to group

```

net localgroup administrators domainname\username /add

net localgroup administrators domainname\username /add
```

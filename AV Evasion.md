## POWERSHELL
Set-MpPreference -DisableRealtimeMonitoring $true;  
Disable Cloud-Based Protection;  
Set-MpPreference -MAPSReporting Disable;  

**ADDING DIRECTORY TO AV EXCLUSION LIST**: Set-MpPreference -ExclusionPath PATH\TO\FOLDER;  




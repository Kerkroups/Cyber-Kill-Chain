## ABUSING LDAP:  
ldapsearch -H ldap://192.168.162.14 -x -b "DC=,DC=" "*" | awk '/dn: / {print $2}'  
ldapsearch -h 172.31.1.4 -x -s base namingcontexts  


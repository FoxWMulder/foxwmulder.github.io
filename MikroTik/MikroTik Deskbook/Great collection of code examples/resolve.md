---
Type of note: Code
Category: MikroTik
title: resolve
Source:
  - "[Nslookup на Mikrotik.](https://mikrotik.moscow/forum/forum57/60843-nslookup-na-mikrotik)"
Tags:
  - mikrotik/scripts
Share: true
---
  
```ad-desc  
collapse: none  
  
В переменной dnsresolveres будет IP в случае удачного его получение.  
  
```  
  
```ad-code  
collapse: none  
title: Code  
  
```pascal  
global dnsresolveres;  
do {  
 global dnsresolveres [:resolve www.microsoft.com];  
 log info "Script 'checkdns': resolve OK";  
} on-error={  
 log warning "Script 'checkdns': error resolve";  
}  
```
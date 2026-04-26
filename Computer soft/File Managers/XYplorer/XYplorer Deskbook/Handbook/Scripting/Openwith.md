---
Type of note:
Category: Computer Soft/XYplorer
title: Openwith
Source:
Tags:
  - soft/xyplorer/commands
  - soft/xyplorer/scripts
Share: true
---
  
  
```ad-desc  
collapse: none  
  
"Открыт с помощью...".  
  
```  
  
```ad-code  
collapse: none  
title: Code  
  
```pascal  
  
openwith """pyw"" ""Y:\Programs\В корзину.pyw""", , "\\?\<curitem>";  
  
```  
  
```ad-code  
collapse: none  
title: Long path  
  
URL: [Custom buttons and long paths](https://www.xyplorer.com/xyfc/viewtopic.php?t=29536)  
  
```js  
  
	if (substr(<curitem>, 0, 2) != "\\") {  
        $longPath = "\\?\" . <curitem>;  
    } else {  
        $longPath = "\\?\UNC\" . trim(<curitem>, "\", "L");  
    }  
    openwith """pyw"" ""$longPath""";  
  
```  
  
```ad-result  
  
  
  
```  
  
```ad-remark  
  
  
  
```  
  
```ad-simcom  
  
  
  
```  
  
  

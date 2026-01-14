---
Type of note:
Category: Computer Soft/Windows
title: Удаление файла hiberfil.sys в Windows 7
Source:
Tags:
  - soft/windows
  - howto
Share: true
---
  
  
1) Turn off  
![](attachments/Pasted%20image%2020251229094828.png)  
  
Мы отключили гибернацию на компьютере и теперь можно удалить файл hiberfil.sys. Наберите Win+R, после чего откроется интерфейс инструмента «Выполнить», в область которого следует вбить:  
  
`powercfg -h off`  
  
Файл удалится сам, может надо перезагрузить.
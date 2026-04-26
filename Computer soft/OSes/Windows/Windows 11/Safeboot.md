---
Type of note:
Category: Computer Soft/Windows
title: Safeboot
Source:
Tags:
  - soft/windows/troubleshooting
Share: true
---
  
  
Cmd > `bcdedit /set {default} safeboot minimal`.  
  
Если Винда не запуускается, это можно сделать через F8 при запуске Винды, оттуда выйти открыть командную строку, выполнить вышеуказанную команду, потом "продолжить запуск". Винда запуститься в safemode.  
  
```ad-desc  
title: Note  
collapse: none  
  
Мне помогла это когда я неудачно попытался создать виртуальный сетовой адаптер. Винда просто не грузилась. Вошёл в safemode, удалил в дистпечерер устройств не удачно созданные адаптеры, перезапустился в обычном режиме.  
  
```
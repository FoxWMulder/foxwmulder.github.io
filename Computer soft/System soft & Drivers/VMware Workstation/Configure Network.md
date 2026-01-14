---
Type of note:
Category: Computer Soft/VMWare
title: Configure Network on VMWare
Source:
Tags:
  - soft/troubleshooting
  - howtoconfigure
Share: true
---
  
  
Чтобы виртуальная машина получила IP от роутера и можно было подключать сетевые диски из локалки, надо:  
зайти в настройки сетевого адаптера VM и выставить режим Bridged  
  
### Если при "Bridge" нет интернета на VM  
  
  
Бриджится **не к тому адаптеру**.  
Edit → Virtual Network Editor → VMnet0  
  
- ❌ Automatic      
- ✅ выбрать **конкретный адаптер**, через который есть интернет    
    (обычно `Wi-Fi` или `Ethernet`, не VPN!)
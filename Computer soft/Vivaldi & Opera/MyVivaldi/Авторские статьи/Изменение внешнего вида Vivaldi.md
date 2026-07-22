---
Type of note:
Category:
title:
Source:
Tags:
Share: true
---
  
### Введение  
  
Постараюсь осветить тут основные моменты которые требуются для того чтобы изменить внешний вид браузера Вивальди под себя.  
  
Интферфейс Вивальди как веб-сайт. Его "страницей" является "window.html" - это HTML-страница с использованием CSS и JavaScript.  
  
Сама "веб-страница" браузера, которая находится в файле "window.html" практически пуста.  
  
### Начинаем  
  
Внешний вид браузера описан в файле "...\Vivaldi\Application\*номер версии*\resources\vivaldi\style\common.css". Но его не стоит редактировать так как он весьма большой и таким образом не получиться переносить внесённые вами правки в новые версии браузера. Стоит сохдать свой CSS-файл с произвольным именем, куда уже прописать все нужные вам стили (правки).  
  
![](attachments/Pasted%20image-2026-07-22%2008-44-37-048.gif)  
  
1. На странице vivaldi://inspect/#apps нажать "Inspect" прямо под надписью "Vivaldi".  
2. В открывшихся окне нажать на эту иконку с курсором  
  
![](attachments/Pasted%20image-2026-07-22%2008-44-37-795.png)  
  
![](attachments/Pasted%20image-2026-07-22%2008-44-38-562.png)  
  
### Внедряем изменения  
  
Открываем "window.html из папки "...\Vivaldi\Application\...\resources\vivaldi" (так где троеточие у вас будет какой-то номер, это версия браузера установленного у вас) и после строчки  
  
`< link rel="stylesheet" href="style/custom.css" / >`  
  
добавляем строчку  
  
`< link rel="stylesheet" href="mycss.css" / > /* вместо "mycss" можно использовать произвольное название, это название вашего файла с CSS-стилями*/`  
  
![](attachments/Pasted%20image-2026-07-22%2008-44-39-284.png)  
  
Vivaldi 2.6 и более свежии версии  
  
1. Открываем vivaldi://experiments  
2. Включаем "Allow for using CSS modifications"  
3. Открываем "Настройки" > "Внешний вид""  
4. Указываем папку в разделе "Свои файлы настройки UI  
5. Кладём свой CSS-файл в эту папку  
6. Перезагружаем браузер и смотрим результ  
  
В написание статьи использовались советы пользователей: [Andz](https://vivaldi.net/en-US/easysocial-dashboard/profile/15939) , [anephew](http://habrahabr.ru/users/anephew/) , [coleslaw](https://forum.vivaldi.net/user/coleslaw) , [sedative29rus](https://forum.vivaldi.net/user/sedative29rus) и [Christoph142](https://forum.vivaldi.net/user/christoph142)
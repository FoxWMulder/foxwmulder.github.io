---
Source:
  - http://www.delphibasics.ru/Packed.php
Tags:
  - delphi/keywords
Share: true
---
```ad-seealso

[Navigation - P](../_Navigation/P.md)
1

```

Local copy: 

```ad-desc
collapse: none

**Unit**: 

**type Name = Packed array[...] of ...;  
type Name = Packed class ... end;  
type Name = Packed object ... end;  
type Name = Packed record ... end;**

Ключевое слово **Packed** говорит Delphi минимизировать память, взятую определенным объектом.  
  
Обычно, сложные типы данных, такие как **записи**, имеют свои элементы по 2, 4 или 8 байта, соответствующие типам данных. Например, поле **Word** было бы 4-байтовое.  
  
Записи также дополняются, для гарантии, что они закончены, 4-х байтовой границей.  
  
Упаковка отменяет это, сжимая данные в наименьшую память, хотя с последующим уменьшенным доступом выполнения.

```

```ad-code
collapse: none
title: Упаковка записи для уменьшения памяти

```pascal
type
  // Объявление распакованной записи
  TDefaultRecord = Record
    name1   : string[4];
    floater : single;
    name2   : char;
    int     : Integer;
  end;

  // Объявление запакованной записи
  TPackedRecord = Packed Record
    name1   : string[4];
    floater : single;
    name2   : char;
    int     : Integer;
  end;

var
  defaultRec : TDefaultRecord;
  packedRec  : TPackedRecord;

begin
  ShowMessage('Размер обычной записи = '+IntToStr(SizeOf(defaultRec)));
  ShowMessage('Размер запакованной записи = '+IntToStr(SizeOf(packedRec)));
end;

```

```ad-result

Размер обычной записи = 20  
Размер запакованной записи = 14

```

```ad-remark

Примеры распакованных данных:  
  
Word     = 2 bytes  
LongWord = 4 bytes  
Single   = 4 bytes  
Double   = 8 bytes

```

```ad-simcom

$Align  Определяет данные будут выровнены или запакованы

[Array](../A/Array)  Тип данных содержащий индексируемую коллекцию данных

[Class](../C/Class)  Начинает объявление типа объектного класса

Object  Позволяет данным типа подпрограмм обращаться к методу объекта

Record  Структурный тип данных содержащий поля данных

```



---
Type of note: Article
Category: Delphi
title: Сравнение скорости добавления текста в SynMemo
Source:
  - "[TMemo is slow when working with large number of lines](https://stackoverflow.com/questions/46990961/tmemo-is-slow-when-working-with-large-number-of-lines)"
Framework:
Platform:
  - Windows
Example: E:\Delphi\Examples\Speed test for functin FileExists etc
Tags:
  - delphi/speedcode
  - delphi/components/synedit
  - delphi/classes/tstringslist
Share: true
---
  
На 8000+ строк следующий код выполняется минуты.  
  
```  
 while i<Count do begin  
  Editor.Lines.Add(list.Strings[i]);  
  Inc(i);  
 end;  
```  
  
Для сравнения:  
  
```  
 var list := TStringList.Create;  
 while i<Count do begin  
  list.Add(list.Strings[i]);  
  Inc(i);  
 end;  
 list.TrailingLineBreak:=False;  
 Editor.BeginUpdate;  
 Editor.Text:=list_temp.Text;  
 Editor.EndUpdate;  
```  
  
Замер скорости выполнения этог кода:  
  
	LoadMutX2 - Editor add text: 4.7124 seconds, 33764281 ticks, call: 1  
  

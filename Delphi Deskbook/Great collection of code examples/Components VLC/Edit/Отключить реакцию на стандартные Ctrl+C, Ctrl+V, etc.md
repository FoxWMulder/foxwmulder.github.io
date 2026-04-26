---
Type of note:
Category: Delphi
title: Отключить реакцию на стандартные Ctrl+C, Ctrl+V, etc
Source:
Framework:
Platform:
Tags:
  - delphi/components/edit
  - delphi/troubleshooting
Share: true
---
  
```ad-code  
collapse: none  
title: Code  
  
```pascal  
procedure TForm1.Edit1KeyPress(Sender: TObject; var Key: Char);  
begin  
if (Key=#22) or (Key=#3) or (key=#24) then begin Key:=#0; end; //ctrl+v, ctrl+c, ctrl+x //чтобы не было реакции на стандартные хоткеи  
end;  
```  

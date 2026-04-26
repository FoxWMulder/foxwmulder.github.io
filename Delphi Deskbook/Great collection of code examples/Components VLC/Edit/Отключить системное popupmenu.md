---
Type of note:
Category: Delphi
title: Отключить системное popupmenu
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
uses Vcl.Menus;  
  
procedure TForm1.FormCreate(Sender: TObject);  
var  
 EmptyPopup: TPopupMenu;  
begin  
EmptyPopup:=TPopupMenu.Create(Self);  
//НЕ добавлять Items — оставить полностью пустым!  
Edit1.PopupMenu:=EmptyPopup;  
end;  
```  
  
Другой вариант:  
  
```ad-code  
collapse: none  
title: Code  
  
```pascal  
  
procedure TForm1.Edit1ContextPopup(Sender: TObject; MousePos: TPoint;  
  var Handled: Boolean);  
begin  
Handled:=True;  
end;  
```  

---
Type of note:
Category: Delphi
title: Получить SMART HDD
Source:
  - "[Delphi SMART SSD/HDD](https://mam-mam.net/delphi/smart.html)"
Framework:
Platform:
  - Windows
Local copy:
  - "[](../../Archive%20of%20web%20pages/Hardware/Delphi%20SMART%20SSD%20or%20HDD%20-(%20mam-mam.net%20)-%20u.zip.html)"
Example: E:\Delphi\Examples\HDD SMART 2
Tags:
  - delphi/hardware/disks
Share: true
---
  
```ad-code  
collapse: none  
title: Code  
  
```pascal  
uses System.Win.Registry;  
  
type  
  TForm1 = class(TForm)  
    Button1: TButton;  
    Memo1: TMemo;  
    procedure Button1Click(Sender: TObject);  
  private  
    { Private declarations }  
  public  
    { Public declarations }  
  end;  
  
type  
  TMamSmart=record  
    AttrID:Byte;  
    StatusFlags:Word;  
    Value:Byte;  
    WorstValue:Byte;  
    Threshold:Byte;  
    RawValue:Array[0..5] of Byte;  
  end;  
  TMamSmartArray=Array of TMamSmart;  
  
var  
  Form1: TForm1;  
  
implementation  
  
const  
  NUMATTRSTRUCTS = 30;  
  SMARTREADATTRIBUTEVALUES = $D0;  
  SMARTREADATTRIBUTETHRESHOLDS = $D1;  
  
  SMARTCYLLOW	= $4F;  
  SMARTCYLHIGH	= $C2;  
  //SMARTCMD Performs SMART cmd.  
  IDEEXECUTESMARTFUNCTION = $B0;  
  //BUFFER SIZE  
  READATTRIBUTEBUFFERSIZE = 512;  
  //DeviceIOControlのAPIへの命令  
  DFPGETVERSION	       = $00074080;  
  DFPSENDDRIVECOMMAND = $0007c084;  
  DFPRECEIVEDRIVEDATA = $0007c088;  
  
type  
  //Attribute情報の構造体  
  DriveAttribute = packed record  
    bAttrID:Byte;  
    wStatusFlags:Word;  
    bAttrValue:Byte;  
    bWorstValue:Byte;  
    bRawValue:array[0..5] of Byte;  
    bReserved:Byte;  
  end;  
  
  //DFPRECEIVEDRIVEDATA命令のINとOUTの構造体  
  IDEREGS=packed record  
    bFeaturesReg:Byte;  
    bSectorCountReg:Byte;  
    bSectorNumberReg:Byte;  
    bCylLowReg:Byte;  
    bCylHighReg:Byte;  
    bDriveHeadReg:Byte;  
    bCommandReg:Byte;  
    bReserved:Byte;  
  end;  
  IDEREGS=IDEREGS;  
  
  SENDCMDINPARAMS=packed record  
    cBufferSize:DWORD;  
    irDriveRegs:IDEREGS;  
    bDriveNumber:Byte;  
    bReserved:array[0..2] of Byte;  
    dwReserved:array[0..3] of DWORD;  
    bBuffer:array[0..0] of Byte;  
  end;  
  SENDCMDINPARAMS=SENDCMDINPARAMS;  
  
  DRIVERSTATUS = packed record  
    bDriveError:Byte;  
    bIDEStatus:Byte;  
    bReserved:array[0..1] of Byte;  
    dwReserved:array[0..1] of DWORD;  
  end;  
  DRIVERSTATUS=DRIVERSTATUS;  
  
  BSENDCMDOUTPARAMS = packed record  
    cBufferSize:DWORD;  
    DriverStatus:DRIVERSTATUS;  
    bBuffer:array [0..511] of Byte;  
  end;  
  BSENDCMDOUTPARAMS=BSENDCMDOUTPARAMS;  
  
  BDriveAttribute = packed record  
    AttrStructRevision:WORD;  
    DA:array[0..NUMATTRSTRUCTS-1] of DriveAttribute;  
  end;  
  PBDriveAttribute =^BDriveAttribute;  
  
  //Threshold情報の構造体  
  AttrThreshold = packed record  
    bAttrID:Byte;  
    bWarrantyThreshold:Byte;  
    bReserved:array[0..9] of Byte;  
  end;  
  BAttrThreshold = packed record  
    ThreStructRevision:Word;  
    AT:array[0..NUMATTRSTRUCTS-1] of AttrThreshold  
  end;  
  PBAttrThreshold =^BAttrThreshold;  
  
{$R *.dfm}  
  
procedure getSmart(DriveNum:Integer;var MamSmartArray:TMamSmartArray);  
var hd:THandle;  
    st:string;  
    wst:array[0..127] of char;  
    br:Cardinal;  
    SCIP:SENDCMDINPARAMS;  
    SCOPATTR:BSENDCMDOUTPARAMS;  
    SCOPTHRE:BSENDCMDOUTPARAMS;  
    PBAttr:PBDriveAttribute;  
    PBThre:PBAttrThreshold;  
    i,ct:Integer;  
begin  
  st:='\\.\PhysicalDrive'+Trim(InttoStr(DriveNum));  
  strpcopy(wst,st);  
  
  //deviceハンドルの取得  
  hd:=CreateFile(wst,GENERICREAD or GENERICWRITE,  
                       FILESHAREREAD or FILESHAREWRITE,  
                       nil, OPENEXISTING, 0, 0);  
  
  if hd = INVALIDHANDLEVALUE then exit;  
  
  ZeroMemory(@SCIP,SizeOf(SENDCMDINPARAMS));  
  ZeroMemory(@SCOPATTR,SizeOf(BSENDCMDOUTPARAMS));  
  
  SCIP.irDriveRegs.bFeaturesReg:=SMARTREADATTRIBUTEVALUES;  
  SCIP.irDriveRegs.bSectorCountReg:=1;  
  SCIP.irDriveRegs.bSectorNumberReg:=1;  
  SCIP.irDriveRegs.bCylLowReg:=SMARTCYLLOW;  
  SCIP.irDriveRegs.bCylHighReg:=SMARTCYLHIGH;  
  SCIP.irDriveRegs.bDriveHeadReg:=$A0 or ((DriveNum and 1) shl 4);  
  SCIP.irDriveRegs.bCommandReg:=IDEEXECUTESMARTFUNCTION;  
  SCIP.cBufferSize:=READATTRIBUTEBUFFERSIZE;  
  SCIP.bDriveNumber:=DriveNum;  
  
  DeviceIoControl(hd,DFPRECEIVEDRIVEDATA,  
    @SCIP,sizeof(SCIP),@SCOPATTR,sizeof(SCOPATTR),br,nil);  
  PBAttr:=PBDriveAttribute(Pointer(@(SCOPATTR.bBuffer)[0]));  
  
  ZeroMemory(@SCIP,SizeOf(SENDCMDINPARAMS));  
  ZeroMemory(@SCOPTHRE,SizeOf(BSENDCMDOUTPARAMS));  
  
  SCIP.irDriveRegs.bFeaturesReg:=SMARTREADATTRIBUTETHRESHOLDS;  
  SCIP.irDriveRegs.bSectorCountReg:=1;  
  SCIP.irDriveRegs.bSectorNumberReg:=1;  
  SCIP.irDriveRegs.bCylLowReg:=SMARTCYLLOW;  
  SCIP.irDriveRegs.bCylHighReg:=SMARTCYLHIGH;  
  SCIP.irDriveRegs.bDriveHeadReg:=$A0 or ((DriveNum and 1) shl 4);  
  SCIP.irDriveRegs.bCommandReg:=IDEEXECUTESMARTFUNCTION;  
  SCIP.cBufferSize:=READATTRIBUTEBUFFERSIZE;  
  SCIP.bDriveNumber:=DriveNum;  
  
  DeviceIoControl(hd,DFPRECEIVEDRIVEDATA,  
    @SCIP,sizeof(SCIP),@SCOPTHRE,sizeof(SCOPTHRE),br,nil);  
  PBThre:=PBAttrThreshold(pointer(@SCOPTHRE.bBuffer));  
  CloseHandle(hd);  
  
  setlength(MamSmartArray,0);  
  ct:=0;  
  for i := 0 to NUMATTRSTRUCTS-1 do  
  begin  
    if PBAttr.DA[i].bAttrID>0 then  
    begin  
      inc(ct);  
      setlength(MamSmartArray,ct);  
      MamSmartArray[ct-1].AttrID:=PBAttr.DA[i].bAttrID;  
      MamSmartArray[ct-1].StatusFlags:=PBAttr.DA[i].wStatusFlags;  
      MamSmartArray[ct-1].Value:=PBAttr.DA[i].bAttrValue;  
      MamSmartArray[ct-1].WorstValue:=PBAttr.DA[i].bWorstValue;  
      MamSmartArray[ct-1].Threshold:=PBThre.AT[i].bWarrantyThreshold;  
      MamSmartArray[ct-1].RawValue[0]:=PBAttr.DA[i].bRawValue[0];  
      MamSmartArray[ct-1].RawValue[1]:=PBAttr.DA[i].bRawValue[1];  
      MamSmartArray[ct-1].RawValue[2]:=PBAttr.DA[i].bRawValue[2];  
      MamSmartArray[ct-1].RawValue[3]:=PBAttr.DA[i].bRawValue[3];  
      MamSmartArray[ct-1].RawValue[4]:=PBAttr.DA[i].bRawValue[4];  
      MamSmartArray[ct-1].RawValue[5]:=PBAttr.DA[i].bRawValue[5];  
    end;  
  end;  
end;  
  
  
procedure TForm1.Button1Click(Sender: TObject);  
var reg:TRegistry;  
    i,j,ct:integer;  
    smart:TMamSmartArray;  
begin  
  ct:=0;  
  //レジストリから物理ドライブ数を取得する  
  reg:=TRegistry.Create(Winapi.Windows.KEYREAD);  
  reg.RootKey:=HKEYLOCALMACHINE;  
  if reg.KeyExists(  
    'SYSTEM\CurrentControlSet\Services\disk\Enum') then  
  begin  
    reg.OpenKey('SYSTEM\CurrentControlSet\Services\disk\Enum',False);  
    ct:=reg.ReadInteger('Count');  
  end;  
  reg.CloseKey;  
  reg.Free;  
  //物理ドライブ数ループ  
  for i := 0 to ct-1 do  
  begin  
    //SMART情報の取得  
    getSmart(i,smart);  
    //SMART情報が取得出来たら  
    if length(smart)>0 then  
    begin  
      memo1.Lines.Add('DriveNum:'+IntToStr(i));  
      //属性数ループ  
      for j := 0 to length(smart)-1 do  
      begin  
        memo1.Lines.Add(  
          format('%3d,%4d,%2d,%2d,%2d,%2.2x%2.2x%2.2x%2.2x%2.2x%2.2x',  
          [ smart[j].AttrID, smart[j].StatusFlags,  
            smart[j].Value,  smart[j].WorstValue, smart[j].Threshold,  
            smart[j].RawValue[5], smart[j].RawValue[4],  
            smart[j].RawValue[3], smart[j].RawValue[2],  
            smart[j].RawValue[1], smart[j].RawValue[0]  
          ])  
        );  
      end;  
    end;  
  end;  
  
end;  
  
end.  
  
```  
  
```ad-result  
  
![](attachments/%D0%9F%D0%BE%D0%BB%D1%83%D1%87%D0%B8%D1%82%D1%8C%20SMART%20HDD/9c640e3652cb649dd990b05815d0ccf3MD5.jpg)  
  
```  
  
```ad-remark  
  
1) Программа должна запускаться с правами адмиистратора.  
     
2) Не получает данные с HDD подключённых через адаптеры и с NVMe-дисков. Для получения информации с NVMe-дисков использовать: [Получить информацию по здоровью NVMe SSD](Получить%20информацию%20по%20здоровью%20NVMe%20SSD)  
  
```  
  
```ad-quo  
collapse: none  
  
URL: https://delphisources.ru/forum/showthread.php?t=25082  
  
а у вас сервис WMI в системе вообще запущен? Он по умолчанию отключен, да и в делфи, как я понял, нужно ещё ОСХ-компонент доустановить.  
  
```  
  
```ad-seealso  
  
[Получить информацию по здоровью NVMe SSD](Получить%20информацию%20по%20здоровью%20NVMe%20SSD)  
  
```
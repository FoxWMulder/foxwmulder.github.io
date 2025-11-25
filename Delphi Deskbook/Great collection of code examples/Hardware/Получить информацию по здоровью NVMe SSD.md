---
Type of note:
Category: Delphi
title: Получить информацию по здоровью NVMe SSD
Source:
  - "[Delphi NVMe Health Log](https://mam-mam.net/delphi/smartnvme.html)"
Framework:
Platform:
  - Windows
Local copy:
  - "[](../../Archive%20of%20web%20pages/Hardware/Delph%20NVMe%20SSD%20SMART%20Health%20Log%20-(%20mam-mam.net%20)-%20u.zip.html)"
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
  
 NVMeStorageQueryProperty =packed record  
    PropertyId:UInt32;    //STORAGEPROPERTYID  
    QueryType:UInt32;     //STORAGEQUERYTYPE  
    ProtocolType:UInt32;  //STORAGEPROTOCOLTYPE  
    DataType:UInt32;      //STORAGEPROTOCOLNVMEDATATYPE  
    ProtocolDataRequestValue:UInt32;  
    ProtocolDataRequestSubValue:UInt32;  
    ProtocolDataOffset:UInt32;  
    ProtocolDataLength:UInt32;  
    FixedProtocolReturnData:UInt32;  
    ProtocolDataRequestSubValue2:UInt32;  
    ProtocolDataRequestSubValue3:UInt32;  
    ProtocolDataRequestSubValue4:UInt32;  
    SMARTData:Array[0..511] of Byte;  
  end;  
  TNVMeStorageQueryProperty=NVMeStorageQueryProperty;  
  
  STORAGEPROPERTYQUERY = packed record  
    PropertyId: UInt32; //STORAGEPROPERTYID  
    QueryType: UInt32;  //STORAGEQUERYTYPE  
    AdditionalParameters: array [0..0] of Byte;  
  end;  
  TStoragePropertyQuery=STORAGEPROPERTYQUERY;  
  
  STORAGEPROTOCOLSPECIFICDATA=packed record  
    ProtocolType:UInt32;//STORAGEPROTOCOLTYPE  
    DataType:UInt32;    //STORAGEPROTOCOLNVMEDATATYPE  
    ProtocolDataRequestValue:UInt32;  
    ProtocolDataRequestSubValue:UInt32;  
    ProtocolDataOffset:UInt32;  
    ProtocolDataLength:UInt32;  
    FixedProtocolReturnData:UInt32;  
    ProtocolDataRequestSubValue2:UInt32;  
    ProtocolDataRequestSubValue3:UInt32;  
    ProtocolDataRequestSubValue4:UInt32;  
  end;  
  TStorageProtocolSpecificData=STORAGEPROTOCOLSPECIFICDATA;  
  PStorageProtocolSpecificData=^TStorageProtocolSpecificData;  
  
  NVMEHEALTHINFOLOG=packed record  
    DUMMYSTRUCTNAME:Byte;  
    Temperature:Array[0..1] of Byte;  
    AvailableSpare:Byte;  
    AvailableSpareThreshold:Byte;  
    PercentageUsed:Byte;  
    Reserved0:array[0..25] of Byte;  
    DataUnitRead:array[0..15] of Byte;  
    DataUnitWritten:array[0..15] of Byte;  
    HostReadCommands:array[0..15] of Byte;  
    HostWrittenCommands:array[0..15] of Byte;  
    ControllerBusyTime:array[0..15] of Byte;  
    PowerCycle:array[0..15] of Byte;  
    PowerOnHours:array[0..15] of Byte;  
    UnsafeShutdowns:array[0..15] of Byte;  
    MediaErrors:array[0..15] of Byte;  
    ErrorInfoLogEntryCount:array[0..15] of Byte;  
    WarningCompositeTemperatureTime:Cardinal;  
    CriticalCompositeTemperatureTime:Cardinal;  
    TemperatureSensor1:Word;  
    TemperatureSensor2:Word;  
    TemperatureSensor3:Word;  
    TemperatureSensor4:Word;  
    TemperatureSensor5:Word;  
    TemperatureSensor6:Word;  
    TemperatureSensor7:Word;  
    TemperatureSensor8:Word;  
    Reserved1:array[0..295] of Byte;  
  end;  
  TNVMeHealthInfoLog=NVMEHEALTHINFOLOG;  
  PNVMeHealthInfoLog=^TNVMeHealthInfoLog;  
  
  STORAGEPROPERTYID = (  
    StorageDeviceProperty = 0,  
    StorageAdapterProperty,  
    StorageDeviceIdProperty,  
    StorageDeviceUniqueIdProperty,  
    StorageDeviceWriteCacheProperty,  
    StorageMiniportProperty,  
    StorageAccessAlignmentProperty,  
    StorageDeviceSeekPenaltyProperty,  
    StorageDeviceTrimProperty,  
    StorageDeviceWriteAggregationProperty,  
    StorageDeviceDeviceTelemetryProperty,  
    StorageDeviceLBProvisioningProperty,  
    StorageDevicePowerProperty,  
    StorageDeviceCopyOffloadProperty,  
    StorageDeviceResiliencyProperty,  
    StorageDeviceMediumProductType,  
    StorageAdapterRpmbProperty,  
    StorageAdapterCryptoProperty,  
    StorageDeviceIoCapabilityProperty = 48,  
    StorageAdapterProtocolSpecificProperty,  
    StorageDeviceProtocolSpecificProperty,  
    StorageAdapterTemperatureProperty,  
    StorageDeviceTemperatureProperty,  
    StorageAdapterPhysicalTopologyProperty,  
    StorageDevicePhysicalTopologyProperty,  
    StorageDeviceAttributesProperty,  
    StorageDeviceManagementStatus,  
    StorageAdapterSerialNumberProperty,  
    StorageDeviceLocationProperty,  
    StorageDeviceNumaProperty,  
    StorageDeviceZonedDeviceProperty,  
    StorageDeviceUnsafeShutdownCount,  
    StorageDeviceEnduranceProperty,  
    StorageDeviceLedStateProperty,  
    StorageDeviceSelfEncryptionProperty = 64,  
    StorageFruIdProperty  
  );  
  
  STORAGEQUERYTYPE = (  
    PropertyStandardQuery = 0,  
    PropertyExistsQuery,  
    PropertyMaskQuery,  
    PropertyQueryMaxDefined  
  );  
  
  STORAGEPROTOCOLTYPE=(  
    ProtocolTypeUnknown = $00,  
    ProtocolTypeScsi,  
    ProtocolTypeAta,  
    ProtocolTypeNvme,  
    ProtocolTypeSd,  
    ProtocolTypeUfs,  
    ProtocolTypeProprietary = $7E,  
    ProtocolTypeMaxReserved = $7F  
  );  
  
  STORAGEPROTOCOLNVMEDATATYPE=(  
    NVMeDataTypeUnknown,  
    NVMeDataTypeIdentify,  
    NVMeDataTypeLogPage,  
    NVMeDataTypeFeature  
  );  
  
var  
  Form1: TForm1;  
  
const  
  NVMENAMESPACEALL:UINT32              =$FFFFFFFF;  
  NVMELOGPAGEERRORINFO               = $01;  
  NVMELOGPAGEHEALTHINFO              = $02;  
  NVMELOGPAGEFIRMWARESLOTINFO       = $03;  
  NVMELOGPAGECHANGEDNAMESPACELIST   = $04;  
  NVMELOGPAGECOMMANDEFFECTS          = $05;  
  NVMELOGPAGEDEVICESELFTEST         = $06;  
  NVMELOGPAGETELEMETRYHOSTINITIATED = $07;  
  NVMELOGPAGETELEMETRYCTLRINITIATED = $08;  
  NVMELOGPAGERESERVATIONNOTIFICATION = $80;  
  NVMELOGPAGESANITIZESTATUS          = $81;  
  NVMELOGPAGEVUSTART                 = $C0;  
  NVMELOGPAGEWCSCLOUDSSD            = $C0;  
  NVMELOGPAGEVUEND                   = $FF;  
  
implementation  
  
{$R *.dfm}  
  
function GetNVMeHealthInfoLog(DriveNum:Integer):TNVMeHealthInfoLog;  
var hd:THandle;  
    st:string;  
    Buffer: array [0..4095] of Byte;  
    //Bufferと同じメモリアドレスを共有する変数を作成  
    SPQuery:TStoragePropertyQuery absolute Buffer;  
    pSPSpecificData:PStorageProtocolSpecificData;  
    BytesReturned:UInt32;  
    pHealthInfoLog:PNVMeHealthInfoLog;  
begin  
  ZeroMemory(@result,SizeOf(TNVMeHealthInfoLog));  
  st:='\\.\PhysicalDrive'+Trim(InttoStr(DriveNum));  
  //deviceハンドルの取得  
  hd:=CreateFile(PChar(st),GENERICREAD or GENERICWRITE,  
    FILESHAREREAD or FILESHAREWRITE, nil, OPENEXISTING, 0, 0  
  );  
  if hd<>INVALIDHANDLEVALUE then  
  begin  
    ZeroMemory(@Buffer[0],Length(Buffer));  
    SPQuery.PropertyId := Ord(STORAGEPROPERTYID.StorageAdapterProtocolSpecificProperty);  
    SPQuery.QueryType := Ord(STORAGEQUERYTYPE.PropertyStandardQuery);  
  
    pSPSpecificData:=@(SPQuery.AdditionalParameters[0]);  
    pSPSpecificData.ProtocolType:=ord(STORAGEPROTOCOLTYPE.ProtocolTypeNvme);  
    pSPSpecificData.DataType:=ord(STORAGEPROTOCOLNVMEDATATYPE.NVMeDataTypeLogPage);  
    pSPSpecificData.ProtocolDataRequestValue:=NVMELOGPAGEHEALTHINFO;  
    pSPSpecificData.ProtocolDataRequestSubValue:=0;  
    pSPSpecificData.ProtocolDataRequestSubValue2:=0;  
    pSPSpecificData.ProtocolDataRequestSubValue3:=0;  
    pSPSpecificData.ProtocolDataRequestSubValue4:=0;  
    pSPSpecificData.ProtocolDataOffset:=SizeOf(STORAGEPROTOCOLSPECIFICDATA);//40  
    pSPSpecificData.ProtocolDataLength:=sizeof(NVMEHEALTHINFOLOG);//512  
    pHealthInfoLog:=  
      @buffer[SizeOf(TStoragePropertyQuery)+  
      Sizeof(TStorageProtocolSpecificData)-1];  
  
    if DeviceIoControl(hd,IOCTLSTORAGEQUERYPROPERTY,  
      @buffer[0],Length(buffer),  
      @buffer[0],Length(Buffer),BytesReturned,nil) then  
    begin  
      CopyMemory(@result,pHealthInfoLog,SizeOf(TNVMeHealthInfoLog));  
    end;  
    CloseHandle(hd);  
  end;  
end;  
  
function toRawValue(b:array of Byte):string;  
var i:Integer;  
begin  
  result:='';  
  for i := Low(b) to High(b) do  
  begin  
    result:=result+Format('%2.2x',[b[i]]);  
  end;  
end;  
  
procedure TForm1.Button1Click(Sender: TObject);  
var reg:TRegistry;  
    i,ct:integer;  
    InfoLog:TNVMeHealthInfoLog;  
begin  
  Memo1.Clear;  
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
  
  for i := 0 to ct-1 do //物理ドライブの数ループ  
  begin  
    InfoLog:=GetNVMeHealthInfoLog(i);  
    if (InfoLog.Temperature[0]<>0) or (InfoLog.Temperature[1]<>0) then  
    begin  
      Memo1.Lines.Add(  
        Format('Temperature:%d℃',  
          [InfoLog.Temperature[0]+(InfoLog.Temperature[1] shl 8)-273]  
        )  
        +#13#10+  
        Format('AvailableSpare:%d%%',[InfoLog.AvailableSpare])  
        +#13#10+  
        Format('AvailableSpareThreshold:%d%%',[InfoLog.AvailableSpareThreshold])  
        +#13#10+  
        Format('PercentageUsed:%d%%',[InfoLog.PercentageUsed])  
        +#13#10+  
        Format('DataUnitRead:%s',[toRawValue(InfoLog.DataUnitRead)])  
        +#13#10+  
        Format('DataUnitWritten:%s',[toRawValue(InfoLog.DataUnitWritten)])  
        +#13#10+  
        Format('HostReadCommands:%s',[toRawValue(InfoLog.HostReadCommands)])  
        +#13#10+  
        Format('HostWrittenCommands:%s',[toRawValue(InfoLog.HostWrittenCommands)])  
        +#13#10+  
        Format('ControllerBusyTime:%s',[toRawValue(InfoLog.ControllerBusyTime)])  
        +#13#10+  
        Format('PowerCycle:%s',[toRawValue(InfoLog.PowerCycle)])  
        +#13#10+  
        Format('PowerOnHours:%s',[toRawValue(InfoLog.PowerOnHours)])  
        +#13#10+  
        Format('UnsafeShutdowns:%s',[toRawValue(InfoLog.UnsafeShutdowns)])  
        +#13#10+  
        Format('MediaErrors:%s',[toRawValue(InfoLog.MediaErrors)])  
        +#13#10+  
        Format('ErrorInfoLogEntryCount:%s',  
          [toRawValue(InfoLog.ErrorInfoLogEntryCount)]  
        )  
        +#13#10+  
        Format('WarningCompositeTemperatureTime:%d',  
          [InfoLog.WarningCompositeTemperatureTime]  
        )  
        +#13#10+  
        Format('CriticalCompositeTemperatureTime:%d',  
          [InfoLog.CriticalCompositeTemperatureTime]  
        )  
      );  
  
      Memo1.Lines.Add(  
        Format('TemperatureSensor1:%d', [InfoLog.TemperatureSensor1])  
        +#13#10+  
        Format('TemperatureSensor2:%d', [InfoLog.TemperatureSensor2])  
        +#13#10+  
        Format('TemperatureSensor3:%d', [InfoLog.TemperatureSensor3])  
        +#13#10+  
        Format('TemperatureSensor4:%d', [InfoLog.TemperatureSensor4])  
        +#13#10+  
        Format('TemperatureSensor5:%d', [InfoLog.TemperatureSensor5])  
        +#13#10+  
        Format('TemperatureSensor6:%d', [InfoLog.TemperatureSensor6])  
        +#13#10+  
        Format('TemperatureSensor7:%d', [InfoLog.TemperatureSensor7])  
        +#13#10+  
        Format('TemperatureSensor8:%d', [InfoLog.TemperatureSensor8])  
      );  
    end;  
  end;  
  
end;  
  
end.  
  
```  
  
  
```ad-result  
  
![](attachments/Pasted%20image-2025-11-25%2011-22-56-576.png)  
  
```  
  
```ad-remark  
  
Программа должна запускаться с правами адмиистратора.  
  
Как вариант выставить в настройках запрос прав администратора ([Manifest](../../IDE/Setting%20project/Manifest)).  
  
![](attachments/Pasted%20image-2025-11-25%2011-26-40-313.png)  
  
```  
  
```ad-seealso  
  
[Получить SMART HDD](Получить%20SMART%20HDD)  
  
```
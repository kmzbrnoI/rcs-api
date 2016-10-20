# MTB library API

Error codes are available [here](error-codes).

All functions and procedures are called by `stdcall`.

##### `function LoadConfig(filename:PChar):Integer`
##### `function SaveConfig(filename:PChar):Integer`

##### `procedure SetLogWrite(state:boolean)`
##### `procedure SetDataInWrite(state:boolean)`
##### `procedure SetDataOutWrite(state:boolean)`

##### `function SetLogWrite():boolean`
##### `function SetDataInWrite():boolean`
##### `function SetDataOutWrite():boolean`

##### `procedure ShowConfigDialog()`
##### `procedure HideConfigDialog()`
##### `procedure ShowAboutDialog()`

##### `function Open():Integer`
##### `function OpenDevice(device:PChar):Integer`
##### `function Close():Integer`
##### `function Start():Integer`
##### `function Stop():Integer`

##### `function GetInput(module, port:Cardinal):Integer`
##### `function GetOutput(module, port:Cardinal):Integer`
##### `function SetOutput(module, port:Cardinal; state:Integer):Integer`

##### `function GetDeviceVersion(version:PChar; versionLen:Cardinal):Integer`
##### `function GetDriverVersion(version:PChar; versionLen:Cardinal):Integer`
##### `function GetLibVersion(version:PChar; versionLen:Cardinal):Integer`

##### `function IsModule(module:Cardinal):Integer`
##### `function GetModuleType(module:Cardinal):Integer`
##### `function GetModuleName(name:PChar; nameLen:Cardinal):Integer`
##### `function GetModuleFW(fw:PChar; fwLen:Cardinal):Integer`
##### `function SetModuleName(name:PChar):Integer`

##### `function SetMtbSpeed(speed:Integer):Integer`
##### `function Opened():Boolean`
##### `function Started():Boolean`

##### `procedure BindBeforeOepn(event:TStdNotifyEvent; data:Pointer)`
##### `procedure BindAfterOpen(event:TStdNotifyEvent; data:Pointer)`
##### `procedure BindBeforeClose(event:TStdNotifyEvent; data:Pointer)`
##### `procedure BindAfterClose(event:TStdNotifyEvent; data:Pointer)`

##### `procedure BindBeforeStart(event:TStdNotifyEvent; data:Pointer)`
##### `procedure BindAfterStart(event:TStdNotifyEvent; data:Pointer)`
##### `procedure BindBeforeStop(event:TStdNotifyEvent; data:Pointer)`
##### `procedure BindAfterStop(event:TStdNotifyEvent; data:Pointer)`

##### `procedure BindOnError(event:TStdErrorEvent; data:Pointer)`
##### `procedure BindOnLog(event:TStdLogEvent; data:Pointer)`

##### `procedure BindOnInputChanged(event:TStdModuleChangeEvent; data:Pointer)`
##### `procedure BindOnOutputChanged(event:TStdModuleChangeEvent; data:Pointer)`

# MTB library API

Error codes are available [here](error-codes).

All functions and procedures are called by `stdcall`.

##### `function LoadConfig(filename:PChar):Integer`

 * Loads library configuration from file `filename`.
 * This function could only be called when device is *closed*.
 * Returns 0 by default.
 * Returns `MTB_FILE_CANNOT_ACCESS` when cannot access configuration file.
 * Returns `MTB_FILE_DEVICE_OPENED` when trying to load confugration with opened
   device.
 * Old configuration is kept when cannot access file.


##### `function SaveConfig(filename:PChar):Integer`

 * Saves current library configuration to file `filename`
 * When `filename` is empty string, configuration is saved to default
   location (`./mtb/data/mtbcfg.ini`)
 * This function can save configuration to another location, however, at the
   next boot, `./mtb/data/mtbcfg.ini` will be loaded.
 * Returns 0 by default.
 * Returns `MTB_FILE_CANNOT_ACCESS` when cannot access configuration file.
 * This function could be called at any time.


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

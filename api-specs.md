# MTB library API

Error codes are available [here](error-codes).

All functions and procedures are called by `stdcall`.

TODO: list callbacks.
TODO: bounds (mtb addrs, ports)

## Configuration files

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


## Logging

##### `procedure SetLogWrite(state:boolean)`

 * Sets wheter to log data into logfile.
 * This option does not affect loging into main application by calling
   `OnLog` event.

##### `procedure SetDataInWrite(state:boolean)`

 * Sets wheter to log raw input data from MTB-USB.
 * This option affects logging into main application via `OnLog` event.


##### `procedure SetDataOutWrite(state:boolean)`

 * Sets wheter to log raw output data to MTB-USB.
 * This option affects logging into main application via `OnLog` event.


##### `function GetLogWrite():boolean`

 * Returns `true` when logging to file enabled, `false` otherwise.


##### `function GetDataInWrite():boolean`

 * Returns `true` when logging raw input data enabled, `false` otherwise.


##### `function GetDataOutWrite():boolean`

 * Returns `true` when logging raw output data enabled, `false` otherwise.


## Dialogs

##### `procedure ShowConfigDialog()`

 * Shows configuration window as non-modal window.


##### `procedure HideConfigDialog()`

 * Hides configuration dialog.


##### `procedure ShowAboutDialog()`

 * Shows about dialog as modal window.


## MTB open/close start/stop

##### `function Open():Integer`

 * Opens default MTB device (= MTB device defined in current configuration).
 * This function performs `open` and starts scanning modules.
 * Calls `BeforeOpen` event when opening starts.
 * When module scanning is finished, `AfterOpen` event is called.
 * Returns 0 by default.
 * Returns `MTB_ALREADY_OPENNED` when device is already openned.
 * Returns `MTB_CANNOT_OPEN_PORT` whet FT open device was unsuccessful
   (more information in log).


##### `function OpenDevice(device:PChar; persist:boolean):Integer`

 * Opens device `device`.
 * When `persist` is `true`, device name is saved to configuration.
 * Simillar to `Open()`.


##### `function Close():Integer`

 * Closes opened device.
 * Calls `BeforeClose` event when closing starts.
 * `AfterClose` event is caled after close. This event is calle always (we
   always manage to close device somehow)!
 * Returns 0 by default.
 * Returns `MTB_NOT_OPENED` when device not opened.


##### `function Opened():Boolean`

 * Returns `true` if MTB device is opened, `false` otherwise.


##### `function Scanned():Boolean`

 * Returns `true` if MTB modules scanning is finished, `false` otherwise.


##### `function Start():Integer`

 * Starts communication with MTB after opening and module scanning.
 * This function calls `BeforeStart` after starting begins.
 * `AfterStart` event is called when communication successfully started.
 * Returns 0 by default.
 * Returns `MTB_ALREADY_STARTED` when communication with MTB already started.
 * Returns `MTB_FIRMWARE_TOO_LOW` when MTB-USB firware is lower than 0.9.20.
   In this case, upgrade is required and Start failes.
 * Returns `MTB_NO_MODULES` when no modules were found. In this case,
   Start function fails.
 * Returns `MTB_OPENING_NOT_FINISHED` when trying to start before module
   scanning is finished. Start function fails.


##### `function Stop():Integer`

 * Stops MTB communication after successfully starting it.
 * This function calls `BeforeStop` after stopping begins.
 * `AfterStop` is called after after communication with MTB is stopped.
   This event is called always.
 * Returns 0 by default.
 * Returns `MTB_NOT_STARTED` when trying to stop non-existing communication.


##### `function Started():Boolean`

 * Returns `true` if MTB communication is enabled, `false` otherwise.


## MTB IO functions

##### `function GetInput(module, port:Cardinal):Integer`

 * This function returns input state of port `port` on mtb module `module`.
 * Returns 0/1 by default.
 * Returns `MTB_MODULE_INVALID_ADDR` when invalid module address is passed
   (out of range).
 * Returns `MTB_MODULE_NOT_AVAILABLE` when `module` was not found on bus.
 * Returns `MTB_MODULE_FAILED` when `module` was available, but got offline.
 * Returns `MTB_PORT_INVALID_NUMBER` when port number is out of bounds.


##### `function GetOutput(module, port:Cardinal):Integer`

 * Returns state of output pin `port` on module `module`.
 * Returns 0-255 by default (higher numbers for scom), 0-1 for MTB-UNI and
   MTB-TTL.
 * Returns `MTB_MODULE_INVALID_ADDR` when invalid module address is passed
   (out of range).
 * Returns `MTB_MODULE_NOT_AVAILABLE` when `module` was not found on bus.
 * Returns `MTB_MODULE_FAILED` when `module` was available, but got offline.
 * Returns `MTB_PORT_INVALID_NUMBER` when port number is out of bounds.


##### `function SetOutput(module, port:Cardinal; state:Integer):Integer`

 * Sets state of output pin `port` on module `module` to state `state`.
 * Returns 0 by default.
 * Returns `MTB_MODULE_INVALID_ADDR` when invalid module address is passed
   (out of range).
 * Returns `MTB_MODULE_NOT_AVAILABLE` when `module` was not found on bus.
 * Returns `MTB_MODULE_FAILED` when `module` was available, but got offline.
 * Returns `MTB_PORT_INVALID_NUMBER` when port number is out of bounds.


## MTB modules

##### `function IsModule(module:Cardinal):Integer`
##### `function GetModuleType(module:Cardinal):Integer`
##### `function GetModuleName(name:PChar; nameLen:Cardinal):Integer`
##### `function GetModuleFW(fw:PChar; fwLen:Cardinal):Integer`
##### `function SetModuleName(name:PChar):Integer`

##### `function SetMtbSpeed(speed:Integer):Integer`


## Library version functions

##### `function GetDeviceVersion(version:PChar; versionLen:Cardinal):Integer`
##### `function GetDriverVersion(version:PChar; versionLen:Cardinal):Integer`
##### `function GetLibVersion(version:PChar; versionLen:Cardinal):Integer`


## Event binders

##### `procedure BindBeforeOpen(event:TStdNotifyEvent; data:Pointer)`
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


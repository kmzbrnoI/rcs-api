RCS API v1.3 Functions Specification
====================================

Error codes are available at `src/Errors.pas`.

All functions and procedures are called by `stdcall`.

Every `PChar` is a pointer to null-terminated UTF-16 string. All strings are
in UTF-16.

## Configuration files

##### `function LoadConfig(filename:PChar):Integer`

 * Loads library configuration from file `filename`.
 * This function could only be called when device is *closed*.
 * Returns 0 by default.
 * Returns `MTB_FILE_CANNOT_ACCESS` when cannot access configuration file.
 * Returns `MTB_FILE_DEVICE_OPENED` when trying to load configuration with opened
   device.
 * Old configuration is kept if cannot access file.
 * Filename `filename` is used for future saving of config.
 * **Caller is recommended to call this function right after API version
   determination.**
 * At start, library can load default config from from its working path, however
   in most cases there would be no config file. The responsibility for setting
   the config files location is not caller.


##### `function SaveConfig(filename:PChar):Integer`

 * Saves current library configuration to file `filename`
 * Returns 0 by default.
 * Returns `MTB_FILE_CANNOT_ACCESS` when cannot access configuration file.
 * This function could be called at any time.


##### `procedure SetConfigFileName(filename:PChar)`

 * Sets filename for next config saving.
 * This function is compulsory, it is not required.


## Logging

##### `procedure SetLogLevel(loglevel:Cardinal)`

 * Sets event-logging loglevel.
 * Loglevels:

```
0 - no logging
1 - errors
2 - warnings
3 - info
4 - commands
5 - raw commands
6 - debug
```


##### `function GetLogLevel():Cardinal`

 * Returns loglevel.


## Dialogs

##### `procedure ShowConfigDialog()`

 * Shows configuration window as non-modal window.
 * This function is not compulsory.
 * Function absence = library has no GUI.


##### `procedure HideConfigDialog()`

 * Hides configuration dialog.
 * This function is not compulsory.
 * Function absence = library has no GUI.


## MTB open/close start/stop

##### `function Open():Integer`

 * Opens default MTB device (= MTB device defined in current configuration).
 * This function performs `open` and starts scanning modules.
 * Calls `BeforeOpen` event when opening starts.
 * When module scanning is finished, `AfterOpen` event is called.
 * Returns 0 by default.
 * Returns `MTB_ALREADY_OPENNED` when device is already opened.
 * Returns `MTB_CANNOT_OPEN_PORT` whet FT open device was unsuccessful
   (more information in log).


##### `function OpenDevice(device:PChar; persist:boolean):Integer`

 * Opens device `device`.
 * When `persist` is `true`, device name is saved to configuration.
 * Similar to `Open()`.


##### `function Close():Integer`

 * Closes opened device.
 * Calls `BeforeClose` event when closing starts.
 * `AfterClose` event is called after close. This event is called always (we
   always manage to close device somehow)!
 * Returns 0 by default.
 * Returns `MTB_NOT_OPENED` when device not opened.
 * Returns `MTB_SCANNING_NOT_FINISHED` when trying to while scanning modules.
   Closing is not permitted while scanning modules.


##### `function Opened():Boolean`

 * Returns `true` if MTB device is opened, `false` otherwise.


##### `function Start():Integer`

 * Starts communication with MTB after opening and module scanning.
 * This function calls `BeforeStart` after starting begins.
 * `AfterStart` event is called when communication successfully started.
 * `OnScanned` event is called after all the inputs are scanned.
 * Returns 0 by default.
 * Returns `MTB_ALREADY_STARTED` when communication with MTB already started.
 * Returns `MTB_FIRMWARE_TOO_LOW` when MTB-USB firmware is lower than 0.9.20.
   In this case, upgrade is required and Start fails.
 * Returns `MTB_NO_MODULES` when no modules were found. In this case,
   Start function fails.
 * Returns `MTB_NOT_OPENED` when trying to start before module
   scanning is finished. Start function fails.
 * Returns `MTB_SCANNING_NOT_FINISHED` when initial scanning of inputs of all
   modules is not finished yet.


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
 * Returns `MTB_NOT_STARTED` when communication with MTB not started.
 * Returns `MTB_MODULE_INVALID_ADDR` when `module` is not available on bus.
 * Returns `MTB_MODULE_FAILED` when `module` was available, but got offline.
 * Returns `MTB_PORT_INVALID_NUMBER` when port number is out of bounds.
 * Returns `MTB_INPUT_NOT_YET_SCANNED` when input port is not yet scanned.


##### `function GetOutput(module, port:Cardinal):Integer`

 * Returns state of output pin `port` on module `module`.
 * Returns 0-255 by default (higher numbers for scom), 0-1 for MTB-UNI and
   MTB-TTL.
 * Returns `MTB_NOT_STARTED` when communication with MTB not started.
 * Returns `MTB_MODULE_INVALID_ADDR` when `module` is not available on bus.
 * Returns `MTB_MODULE_FAILED` when `module` was available, but got offline.
 * Returns `MTB_PORT_INVALID_NUMBER` when port number is out of bounds.


##### `function SetOutput(module, port:Cardinal; state:Integer):Integer`

 * Sets state of output pin `port` on module `module` to state `state`.
 * Returns 0 by default.
 * Returns `MTB_NOT_STARTED` when communication with MTB not started.
 * Returns `MTB_MODULE_INVALID_ADDR` when `module` is not available on bus.
 * Returns `MTB_MODULE_FAILED` when `module` was available, but got offline.
 * Returns `MTB_PORT_INVALID_NUMBER` when port number is out of bounds.
 * Returns `MTB_INVALID_SCOM_CODE` when scom code is not in 0 to 15.


##### `function GetInputType(module, port:Cardinal):Integer`

 * This function returns type of inpput of port `port` on mtb module `module`.
 * Returns 0 by default.
 * Return value:
    - 0 = plain input
    - 1 = IR
 * Returns `MTB_MODULE_INVALID_ADDR` when `module` is not available on bus.
 * Returns `MTB_PORT_INVALID_NUMBER` when port number is out of bounds.

##### `function GetOutputType(module, port:Cardinal):Integer`

 * This function returns type of output of port `port` on mtb module `module`.
 * Returns 0 by default.
 * Return value:
    - 0 = plain output
    - 1 = SCom
 * Returns `MTB_MODULE_INVALID_ADDR` when `module` is not available on bus.
 * Returns `MTB_PORT_INVALID_NUMBER` when port number is out of bounds.


## MTB-USB board

##### `function GetDeviceCount():Integer`

 * Returns amount of connected MTB-USB boards.


##### `procedure GetDeviceSerial(index:Integer, serial:PChar, maxSerialLen:Cardinal)`

 * Returns serial name of MTB-USB device at index `index` into `serial`.
 * When invalid index is passed, empty string is returned.
 * `serial` should have space for at least 32 bytes.


## MTB modules

##### `function IsModule(module:Cardinal):Boolean`

 * Returns `true` if and only if mtb module with address `module` is fully
   operable now.
 * `IsModule(failed_module) = false`.


##### `function IsModuleFailure(module:Cardinal):Boolean`

 * Returns `true` for modules, which are in *failed* state (i. e. modules
   which were discovered, but failed during communication – this could
   happen when modules are cut of a electricity).


##### `function GetModuleCount():Cardinal`

 * Returns amount of MTB modules discovered.
 * Returns 0 when bus is not scanned or being scanned.
 * Returns 0 after close.


##### `function GetMaxModuleAddr():Cardinal`

 * Returns highest possible module address.
 * Return value of this function should keep constant for whole lifetime of
   the library.

##### `function GetModuleTypeStr(module:Cardinal; type:PChar; maxTypeLen:Cardinal):Integer`

 * Returns type of a module with address `module` as string into `type`.
 * Returns 0 by default.
 * Returns `MTB_MODULE_INVALID_ADDR` when module does not exist.


##### `function GetModuleName(module:Cardinal; name:PChar; maxNameLen:Cardinal):Integer`

 * Puts name of a module `module` into `name`.
 * Returns 0 by default.
 * Returns `MTB_MODULE_INVALID_ADDR` when module address is out of range.
 * `name` should have space for at least 32 bytes.


##### `function GetModuleFW(module:Cardinal; fw:PChar; maxFwLen:Cardinal):Integer`

 * Puts firmware version of module `module` into `fw`.
 * Returns 0 by default.
 * Returns `MTB_MODULE_INVALID_ADDR` when module was not found on the bus.
 * `fw` should have space for at least 16 bytes.


##### `function GetModuleInputsCount(module:Cardinal):Cardinal`

 * Returns number of inputs of module with address `module`.
 * If module does not exist, returns expected number of inputs of module
   with address `module`.
 * Result value can range in `0-15`, other values are forbidden!
 * Returns `MTB_MODULE_INVALID_ADDR` iff `module > GetMaxModuleAddr()`.
 * Number of inputs of a module can change during communication.
 * When a library does not provide this function, it is assumed every active
   module has 16 inputs (0-15).


##### `function GetModuleOutputsCount(module:Cardinal):Cardinal`

 * Returns number of outputs of module with address `module`.
 * If module does not exist, returns expected number of outputs of module
   with address `module`.
 * Result value can range in `0-15`, other values are forbidden!
 * Returns `MTB_MODULE_INVALID_ADDR` iff `module > GetMaxModuleAddr()`.
 * Number of outputs of a module can change during communication.
 * When a library does not provide this function, it is assumed every active
   module has 16 outputs (0-15).


## Library version functions

API version is `Cardinal` (`unsigned int`) with LSB meaning the major version
and seconds LSB meaning minor version. The rest of bytes is 0.

Library may support multiple version of API and caller may support multiple
version of API too. It is caller's responsibility to choose which version of
API to use. Caller determines which versions of API library supports by
calling `ApiSupportsVersion` functions. When it chooses API version, it should
call `ApiSetVersion` so the library knows the intended version of API too.
These functions should be the first functions caller calls in the library.
When these functions are not implemented, `v1.2` of library is assumed.

##### `function ApiSupportsVersion(version:Cardinal):Boolean`

 * Asks library if it supports API version `version`.
 * Return value = if it supports the version or not


##### `function ApiSetVersion(version:Cardinal):Integer`

 * Sets the API version.
 * Should be called at least once at the beginning.
 * Returns 0 on success.
 * Returns `MTB_UNSUPPORTED_API_VERSION` when tried to set unsupported API
   version.


##### `function GetDeviceVersion(version:PChar; maxVersionLen:Cardinal):Integer`

 * Puts version of MTB-USB device into `version`.
 * Library must be connected to MTB-USB device to make this information
   available.
 * Returns 0 by default.
 * Returns `MTB_DEVICE_DISCONNECTED` when not connected to MTB-USB device.
 * `version` should have space for at least 32 bytes.


##### `procedure GetDriverVersion(version:PChar; maxVersionLen:Cardinal)`

 * Puts version of MTB driver into `version`.
 * `version` should have space for at least 32 bytes.


## Event binders

 * Each of the following functions allows parent application to specify its own
   callback function which is called when certain event happens.
 * E. g. by calling `BindBeforeOpen(myFunc, nil)` you specify, that `myFunc`
   should be called before device is opened. `data` could be any valid pointer.
   This pointer is forwarded into callback function as its parameter.
 * Calling `BindBeforeOpen(nil, nil)` disables callback.

```pascal
TStdNotifyEvent = procedure (Sender: TObject; data:Pointer); stdcall;
TStdLogEvent = procedure (Sender: TObject; data:Pointer; logLevel:Integer; msg:PChar); stdcall;
TStdErrorEvent = procedure (Sender: TObject; data:Pointer; errValue: Word; errAddr: Cardinal; errMsg:PChar); stdcall;
TStdModuleChangeEvent = procedure (Sender: TObject; data:Pointer; module: Cardinal); stdcall;
```

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

##### `procedure BindOnScanned(event:TStdNotifyEvent; data:Pointer)`

## Changelog

Whole version history is available in git history of this wiki (see tags)

### v1.3

 * Add API version functions & workflow.
 * Events use `Cardinal` for module address instead of `Byte`.
 * Change the way config file paths are handled (caller should determine config
   file location).
 * Add `SetConfigFileName` procedure.
 * Make `ShowConfigDialog` & `HideConfigDialog` optional.
 * Add `GetMaxModuleAddr` function.
 * `GetModule*Count`: do not return `MTB_MODULE_INVALID_ADDR` when module not
   available on bus.
 * Change meaning of log levels.

### v1.2

 * Update error codes.
 * Minor fixes.
 * Explain some error codes.

### v1.1

 * Add request for number of IO pins per module.
 * `GetModuleType` → `GetModuleTypeStr`

### v1.0

 * Initial version of API.
RCS API v1.5 Functions Specification
====================================

All functions and procedures are called by `stdcall`.

Every `PChar` is a pointer to null-terminated UTF-16 string. All strings are
in UTF-16.

## Configuration files

##### `function LoadConfig(filename:PChar):NativeInt`

 * Loads library configuration from file `filename`.
 * This function could only be called when device is *closed*.
 * Returns 0 by default.
 * Returns `RCS_FILE_CANNOT_ACCESS` when cannot access configuration file.
 * Returns `RCS_FILE_DEVICE_OPENED` when trying to load configuration with opened
   device.
 * Old configuration is kept if cannot access file.
 * Filename `filename` is used for future saving of config.
 * **Caller is recommended to call this function right after API version
   determination.**
 * At start, library can load default config from from its working path, however
   in most cases there would be no config file. The responsibility for setting
   the config files location is not caller.

##### `function SaveConfig(filename:PChar):NativeInt`

 * Saves current library configuration to file `filename`
 * Returns 0 by default.
 * Returns `RCS_FILE_CANNOT_ACCESS` when cannot access configuration file.
 * This function could be called at any time.

##### `procedure SetConfigFileName(filename:PChar)`

 * Sets filename for next config saving.
 * This function is not compulsory.


## Logging

##### `procedure SetLogLevel(loglevel:NativeUInt)`

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

##### `function GetLogLevel():NativeUInt`

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


## RCS open/close start/stop

##### `function Open():NativeInt`

 * Opens default RCS device (= RCS device defined in current configuration).
 * This function performs `open` and starts scanning modules.
 * Calls `BeforeOpen` event when opening starts.
 * When module scanning is finished, `AfterOpen` event is called.
 * Returns 0 by default.
 * Returns `RCS_ALREADY_OPENNED` when device is already opened.
 * Returns `RCS_CANNOT_OPEN_PORT` whet FT open device was unsuccessful
   (more information in log).

##### `function Close():NativeInt`

 * Closes opened device.
 * Calls `BeforeClose` event when closing starts.
 * `AfterClose` event is called after close. This event is called always (we
   always manage to close device somehow)!
 * Returns 0 by default.
 * Returns `RCS_NOT_OPENED` when device not opened.
 * Returns `RCS_SCANNING_NOT_FINISHED` when trying to while scanning modules.
   Closing is not permitted while scanning modules.

##### `function Opened():Boolean`

 * Returns `true` if RCS device is opened, `false` otherwise.

##### `function Start():NativeInt`

 * Starts communication with RCS after opening and module scanning.
 * This function calls `BeforeStart` after starting begins.
 * `AfterStart` event is called when communication successfully started.
 * `OnScanned` event is called after all the inputs are scanned.
 * Returns 0 by default.
 * Returns `RCS_ALREADY_STARTED` when communication with RCS already started.
 * Returns `RCS_FIRMWARE_TOO_LOW` when RCS-USB firmware is lower than 0.9.20.
   In this case, upgrade is required and Start fails.
 * Returns `RCS_NOT_OPENED` when trying to start before module
   scanning is finished. Start function fails.
 * Returns `RCS_SCANNING_NOT_FINISHED` when initial scanning of inputs of all
   modules is not finished yet.

##### `function Stop():NativeInt`

 * Stops RCS communication after successfully starting it.
 * This function calls `BeforeStop` after stopping begins.
 * `AfterStop` is called after after communication with RCS is stopped.
   This event is called always.
 * Returns 0 by default.
 * Returns `RCS_NOT_STARTED` when trying to stop non-existing communication.

##### `function Started():Boolean`

 * Returns `true` if RCS communication is enabled, `false` otherwise.


## RCS IO functions

##### `function GetInput(module, port:NativeUInt):NativeInt`

 * This function returns input state of port `port` on RCS module `module`.
 * Returns 0/1 by default.
 * Returns `RCS_NOT_STARTED` when communication with RCS not started.
 * Returns `RCS_MODULE_INVALID_ADDR` when `module` is not available on bus.
 * Returns `RCS_MODULE_FAILED` when `module` was available, but got offline.
 * Returns `RCS_PORT_INVALID_NUMBER` when port number is out of bounds.
 * Returns `RCS_INPUT_NOT_YET_SCANNED` when input port is not yet scanned.

##### `function SetOutput(module, port:NativeUInt; state:NativeInt):NativeInt`

 * Sets state of output pin `port` on module `module` to state `state`.
 * Returns 0 by default.
 * Returns `RCS_NOT_STARTED` when communication with RCS not started.
 * Returns `RCS_MODULE_INVALID_ADDR` when `module` is not available on bus.
 * Returns `RCS_MODULE_FAILED` when `module` was available, but got offline.
 * Returns `RCS_PORT_INVALID_NUMBER` when port number is out of bounds.
 * Returns `RCS_PORT_INVALID_VALUE` when `state` is not of allowed value.

Outputs state:
 * Plain output:
   - 0: off, 1: on
   - 60, 120, 180, 240, 300, 600, 33, 66: flickering (ticks per minute)
 * S-COM: S-COM code


##### `function GetOutput(module, port:NativeUInt):NativeInt`

 * Returns state of output pin `port` on module `module`.
 * Returns 0-255 by default (higher numbers for scom), 0-1 for RCS-UNI and
   RCS-TTL.
 * Returns `RCS_NOT_STARTED` when communication with RCS not started.
 * Returns `RCS_MODULE_INVALID_ADDR` when `module` is not available on bus.
 * Returns `RCS_MODULE_FAILED` when `module` was available, but got offline.
 * Returns `RCS_PORT_INVALID_NUMBER` when port number is out of bounds.


##### `function SetInput(module, port:NativeUInt; state:NativeInt):NativeInt`

 * Allows to simulate chage of states of inputs.
 * Returns 0 by default.
 * Returns `RCS_NOT_STARTED` when communication with RCS not started.
 * Returns `RCS_MODULE_INVALID_ADDR` when `module` is not available on bus.
 * Returns `RCS_MODULE_FAILED` when `module` was available, but got offline.
 * Returns `RCS_PORT_INVALID_NUMBER` when port number is out of bounds.
 * Returns `RCS_PORT_INVALID_VALUE` when `state` is not of allowed value.

##### `function IsSimulation():Boolean`

 * Returns if this library supports SetInput.
 * This function is not compulsory.

##### `function GetInputType(module, port:NativeUInt):NativeInt`

 * This function returns type of inpput of port `port` on RCS module `module`.
 * Returns 0 by default.
 * Return value:
    - 0 = plain input
    - 1 = IR
 * Returns `RCS_MODULE_INVALID_ADDR` when `module` is not available on bus.
 * Returns `RCS_PORT_INVALID_NUMBER` when port number is out of bounds.

##### `function GetOutputType(module, port:NativeUInt):NativeInt`

 * This function returns type of output of port `port` on RCS module `module`.
 * Returns 0 by default.
 * Return value:
    - 0 = plain output
    - 1 = S-COM
 * Returns `RCS_MODULE_INVALID_ADDR` when `module` is not available on bus.
 * Returns `RCS_PORT_INVALID_NUMBER` when port number is out of bounds.


## RCS modules

##### `function IsModule(module:NativeUInt):Boolean`

 * Returns `true` if and only if RCS module with address `module` is fully
   operable now.
 * `IsModule(failed_module) = false`.

##### `function IsModuleFailure(module:NativeUInt):Boolean`

 * Returns `true` for modules, which are in *failed* state (i. e. modules
   which were discovered, but failed during communication – this could
   happen when modules are cut of a electricity).

##### `function GetModuleCount():NativeUInt`

 * Returns amount of RCS modules discovered.
 * Returns 0 when bus is not scanned or being scanned.
 * Returns 0 after close.

##### `function GetMaxModuleAddr():NativeUInt`

 * Returns highest possible module address.
 * Return value of this function should keep constant for whole lifetime of
   the library.

##### `function GetModuleTypeStr(module:NativeUInt; type:PChar; maxTypeLen:NativeUInt):NativeInt`

 * Returns type of a module with address `module` as string into `type`.
 * Returns 0 by default.
 * Returns `RCS_MODULE_INVALID_ADDR` when module does not exist.

##### `function GetModuleName(module:NativeUInt; name:PChar; maxNameLen:NativeUInt):NativeInt`

 * Puts name of a module `module` into `name`.
 * Returns 0 by default.
 * Returns `RCS_MODULE_INVALID_ADDR` when module address is out of range.
 * `name` should have space for at least 32 bytes.

##### `function GetModuleFW(module:NativeUInt; fw:PChar; maxFwLen:NativeUInt):NativeInt`

 * Puts firmware version of module `module` into `fw`.
 * Returns 0 by default.
 * Returns `RCS_MODULE_INVALID_ADDR` when module was not found on the bus.
 * `fw` should have space for at least 16 bytes.

##### `function GetModuleInputsCount(module:NativeUInt):NativeUInt`

 * Returns number of inputs of module with address `module`.
 * If module does not exist, returns expected number of inputs of module
   with address `module`.
 * Result value can range in `0-15`, other values are forbidden!
 * Returns `RCS_MODULE_INVALID_ADDR` iff `module > GetMaxModuleAddr()`.
 * Number of inputs of a module can change during communication.
 * When a library does not provide this function, it is assumed every active
   module has 16 inputs (0-15).

##### `function GetModuleOutputsCount(module:NativeUInt):NativeUInt`

 * Returns number of outputs of module with address `module`.
 * If module does not exist, returns expected number of outputs of module
   with address `module`.
 * Result value can range in `0-15`, other values are forbidden!
 * Returns `RCS_MODULE_INVALID_ADDR` iff `module > GetMaxModuleAddr()`.
 * Number of outputs of a module can change during communication.
 * When a library does not provide this function, it is assumed every active
   module has 16 outputs (0-15).

##### `function IsModuleError(module:NativeUInt):Boolean`

 * Returns `true` for modules, which reported that they are in *error* state.
 * Returns `false` for non-existing module.

##### `function IsModuleWarning(module:NativeUInt):Boolean`

 * Returns `true` for modules, which reported that they are in *warning* state.
 * Returns `false` for non-existing module.


## Library version functions

API version is `NativeUInt` (`unsigned int`) with LSB meaning the major version
and seconds LSB meaning minor version. The rest of bytes is 0.

Library may support multiple version of API and caller may support multiple
version of API too. It is caller's responsibility to choose which version of
API to use. Caller determines which versions of API library supports by
calling `ApiSupportsVersion` functions. When it chooses API version, it should
call `ApiSetVersion` so the library knows the intended version of API too.
These functions should be the first functions caller calls in the library.
When these functions are not implemented, `v1.2` of library is assumed.

##### `function ApiSupportsVersion(version:NativeUInt):Boolean`

 * Asks library if it supports API version `version`.
 * Return value = if it supports the version or not

##### `function ApiSetVersion(version:NativeUInt):NativeInt`

 * Sets the API version.
 * Should be called at least once at the beginning.
 * Returns 0 on success.
 * Returns `RCS_UNSUPPORTED_API_VERSION` when tried to set unsupported API
   version.

##### `function GetDeviceVersion(version:PChar; maxVersionLen:NativeUInt):NativeInt`

 * Puts version of RCS-USB device into `version`.
 * Library must be connected to RCS-USB device to make this information
   available.
 * Returns 0 by default.
 * Returns `RCS_DEVICE_DISCONNECTED` when not connected to RCS-USB device.
 * `version` should have space for at least 32 bytes.

##### `procedure GetDriverVersion(version:PChar; maxVersionLen:NativeUInt)`

 * Puts version of RCS driver into `version`.
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
TStdLogEvent = procedure (Sender: TObject; data:Pointer; logLevel:NativeInt; msg:PChar); stdcall;
TStdErrorEvent = procedure (Sender: TObject; data:Pointer; errValue: Word; errAddr: NativeUInt; errMsg:PChar); stdcall;
TStdModuleChangeEvent = procedure (Sender: TObject; data:Pointer; module: NativeUInt); stdcall;
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

 * `OnInputChanged` event is called when module inputs change.
 * Also called when module lost / restored (because inputs change).

##### `procedure BindOnOutputChanged(event:TStdModuleChangeEvent; data:Pointer)`

 * `OnOutputChanged` event is called when module outputs change.
 * Also called when module lost / restored (because outputs change).

##### `procedure BindOnModuleChanged(event:TStdModuleChangeEvent; data:Pointer)`

 * `OnModuleChanged` event is called when module general information is changed
   (e.g. name, n.o. inputs/outputs etc.).
 * Also called when module lost / restored.
 * This event could be called during normal RCS operation (RCS active).

##### `procedure BindOnScanned(event:TStdNotifyEvent; data:Pointer)`

 * This event is called after calling `Start()` when state of all inputs is
   loaded.

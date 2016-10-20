# MTB library wiki

This wiki describes functionality of MTB.dll dynamic library and its API.

MTB library is used to set and get state of [MTB modules](http://mtb.kmz-brno.cz)
on model railroad via simple dll interface. It supports MTB-UNI and MTB-TTL
modules with firmware 4.1.

This library could be loaded from any program (be sure to match library and
application architecture [e.g. x86 or x86-64]). MTB.dll is currently distributed
only in x86 (32-bit) architecture.

The library is implemented in Object Pascal and usually being compiled in
Delphi 2009 32-bit.

The library has windows, however, it is fully operable withnout using them.

## Requirements

 * Fully operable [MTB-USB module](http://mtb.kmz-brno.cz/modul_usb.htm)
   connected to PC via USB.
 * MTB-USB driver (available in project releases). Driver is custom FTDI driver,
   installation from unknown sources is required to install this driver.

## Sample workflow

 1.  Connect MTB-USB board via USB.
 2.  Install drivers.
 3.  Load the library into you program.
 4.  Select one of available MTB-USB boards.
 5.  Open device (by calling `Open` from your app).
 6.  Optional: configure modules (configuration will be saved).
 7.  Start communication (by calling `Start` from your app).
 8.  Use the bus (`SetOutput`, `GetInput`, ...).
 9.  Stop communication by calling `Stop`.
 10. Close device by calling `Close`.

## API specification

API specification is available [here](api-specs).

## Configuration file specification

Library configuration file specification is available [here](config).

## Library workflow

 * When initializezd, library tries to load default configuration file
   placed at `./mtb/data/mtbcfg.ini`.
 * All logs are placed by default to file `./mtb/log/date.log`. This setting
   could be changed in configuration file.
 * Logging can be enabled/disabled in configuration file.
 * Library automatically saves its configuration into `./mtb/data/mtbcfg.ini`
   before exiting.

## Licence

This software is distributed as open source under Apache License v2.0.

## Authors

This library bases on MTB component for Delphi created by Petr Travnik.

This library was created by:
 * Jan Horacek (jan.horacek@kmz-brno.cz) (current maintainer)
 * Michal Petrilak (engineercz@gmail.com)

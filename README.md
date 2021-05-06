RCS API v1.3 Specification
==========================

This document describes a specification of *Railroad Control System* Interface
API. This API is used between [hJOPserver](https://github.com/kmzbrnoI/hJOPserver)
and systems for controlling railroad accessories (e.g. MTB, XpressNET etc.).

---

Every railroad control system `dll` library should implement API described in
this document to be used from hJOP.

 * [Functions specification](functions.md)
 * [Error codes description](errors.md)
 * [Changelog](changelog.md)


## Expected RCS workflow

 1. `Open` – open RCS hardware, scan available modules.
 2. `Start` – scan inputs state.
 3. RCS in running.
 4. `Stop` – stop scanning inputs.
 5. `Close` – close connection to RCS hardware.

## See also

 * [hJOP](https://hjop.kmz-brno.cz/) (in Czech)
 * [hJOP RCS](https://hjop.kmz-brno.cz/rcs) (in Czech)
 * [hJOPserver](https://github.com/kmzbrnoI/hJOPserver) (implements RCS API)
 * [RCS Delphi](https://github.com/kmzbrnoI/rcs-delphi) (used by hJOPserver)
 * [MTB direct library](https://github.com/kmzbrnoI/mtb-lib) (implements RCS API)
 * [MTB v4 network library](https://github.com/kmzbrnoI/mtb-net-lib) (implements RCS API)
 * [XpressNET RCS Library](https://github.com/kmzbrnoI/rcs-lib-XpressNET-qt) (implements RCS API)

## Licence

Content of the repository is provided under [Creative Commons
Attribution-ShareAlike 4.0
License](https://creativecommons.org/licenses/by-sa/4.0/).

## Authors

 * Jan Horacek (jan.horacek@kmz-brno.cz) (current maintainer)
 * Michal Petrilak

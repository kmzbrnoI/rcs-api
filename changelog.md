RCS API Changelog
=================

Whole version history is available in git history of this repository (see tags).

## v1.3

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

## v1.2

 * Update error codes.
 * Minor fixes.
 * Explain some error codes.

## v1.1

 * Add request for number of IO pins per module.
 * `GetModuleType` → `GetModuleTypeStr`

## v1.0

 * Initial version of API.

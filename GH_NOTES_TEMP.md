### Fixed
- **V3 shutdown hangs on RS485 disable**: The shutdown sequence wrote `0` to register 42000 to disable RS485 control mode, but the firmware only accepts `0x55BB` (21947). The invalid value caused a Modbus Exception 3 (Illegal Data Value) with 40s+ timeout, blocking integration reload. Now writes the correct `command_off` value (0x55BB).
- **V3 work mode manipulation removed**: Removed V3-specific code that set `user_work_mode` to Manual (0) on setup and restored to Auto (1) on shutdown via register 43000. V3 batteries operate the same as V2 and do not require work mode changes. This also eliminates the state conflict that caused the RS485 disable to fail during shutdown.

### Removed
- `Working Mode` select entity for V3 batteries (was exposed as Manual / Anti-Feed / Trade Mode). The register is not needed for integration control.
- `user_work_mode` entry from V3 register map (set to `None`, matching V2).
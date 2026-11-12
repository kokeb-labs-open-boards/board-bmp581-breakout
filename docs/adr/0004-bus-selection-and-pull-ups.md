# ADR-0004: Bus selection, address and pull-ups

- Status: proposed
- Date: 2026-11-11
- Affects: N-001, N-008, R-002, R-016, H-004

## Context

The chip picks I2C or SPI from CSB at power-up: held at VDDIO from power-up selects I2C (datasheet 5.1, p. 31). On I2C,
SDO sets the address: VSS gives 0x46, VDDIO gives 0x47 (datasheet 5.6, p. 38). SDO and CSB must never float on I2C.
The driver's reference circuits (docs/hardware.yml) add I2C pull-ups of 4.7 kOhm to VDDIO and, for SPI, a 47 kOhm
pull-up on MISO so the frame that selects SPI reads 0xFF. Both values are marked unverified there: the datasheet
figures show no pull-ups.

## Options considered

| Option | For | Against |
| --- | --- | --- |
| A. Pin-header jumpers (2.54 mm) | Easy to change; visible | Big; each jumper is a place to lose a shunt |
| B. Solder jumpers, default bridged | Small; reliable; no loose parts | Needs a knife or iron to change |
| C. DIP switch | One clean control | Bigger; a switch contact in a signal path |

## Decision

Option B, following the default in R-016: I2C, address 0x47, INT on the header. The solder jumpers are:

- **Bus mode:** CSB to VDDIO (I2C, default) or to the header pin for SPI chip select.
- **Address / SPI data out:** SDO to VDDIO (0x47, default), to VSS (0x46), or open to the header for SPI MISO.
- **Pull-ups:** SCK and SDI pull-ups to VDDIO, on by default and cut for SPI; a MISO pull-up used only in SPI mode.

The values of the pull-ups follow the driver's circuits until the bench shows otherwise (V-003); the board does not
call them verified.

## Consequences

- H-004 is controlled by the default and by the documented jumper table (R-002, R-016); V-010 tests the default.
- In SPI mode the user cuts three jumpers and bridges others; the documentation has one table for it.
- Two boards on one I2C chain need different addresses, so one has the SDO jumper moved.
- The schematic review checks the jumper states against R-002 line by line.

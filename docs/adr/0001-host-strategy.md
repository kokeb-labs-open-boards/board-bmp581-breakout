# ADR-0001: The base board is standalone, with no MCU

- Status: proposed
- Date: 2026-11-11
- Affects: N-001, N-003, N-009, R-001, R-004, `board.yml` (`host`, `variants`)

## Context

N-001 asks for the driver to run on the real chip. The driver is MCU-agnostic. Its only platform with a port and a
CI-built example is ESP32 (ESP-IDF); there is a host simulation, and no STM32, RP2040 or nRF port yet
([driver platforms](https://github.com/kokeb-labs-drivers/driver-bmp581/blob/main/docs/platforms.md)). Engineers who
evaluate a sensor already have a host board, and they differ in which one.

## Options considered

| Option | For | Against |
| --- | --- | --- |
| A. Standalone breakout | Works with any host; the smallest board; nothing to certify or support beyond the sensor | The user must bring a host |
| B. Carrier for an MCU module | Reusable for other sensors | Adds a module socket and a pin map to maintain; the first board is not the place for it |
| C. Board with an MCU (ESP32-S3) | Same-day start for people without a host (N-009) | MCU, USB, antenna and firmware to design, support and re-qualify; one platform only |

## Decision

Rev A of `bmp581-breakout` is option A, variant `base`. N-009 stays `deferred`. A variant with an MCU is added only
after the base board has been verified, and starts with the platform that already has a CI-built driver example
(see ADR-0006).

## Consequences

- The board verifies the driver on whatever host the tester brings, which is what N-001 needs.
- `board.yml` keeps `host: standalone` and `mcu: null`; one variant, `base`.
- R-004 (the driver runs unmodified) is verified on the hosts named in the verification record, not on every MCU.
- A later variant gets its own requirements and ADR; it does not change the base board's trace.

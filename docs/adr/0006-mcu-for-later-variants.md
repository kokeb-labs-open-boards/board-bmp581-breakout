# ADR-0006: Which MCU a later variant starts with

- Status: proposed
- Date: 2026-11-11
- Affects: N-009 (deferred), `board.yml` (`variants`, `mcu`)

## Context

ADR-0001 keeps rev A standalone. When a variant with an MCU is wanted (N-009), the choice should follow evidence, not
taste. The board stays a supplier component for people who build their own products; the MCU is a reference host,
not a claim about what an integrator should use.

## Criteria

| Criterion | Why it matters |
| --- | --- |
| A driver port and a CI-built example exist | The variant verifies the driver unmodified (R-004) |
| SPI at up to 12 MHz and 1.8 V to 3.3 V I/O | The chip allows both (driver-bmp581 `hardware.yml`) |
| Programming and debugging over one USB connector | Same-day start (N-009) |
| Long-term supply and a published lifecycle status | An integrator may copy the design (N-006) |
| Familiarity in the integrators' world | A reference they recognise is easier to adopt |
| Wireless or not | Adds antenna, certification and security support; not needed to verify a sensor |

## Options considered

| Option | For | Against |
| --- | --- | --- |
| ESP32-S3 module | The only platform with a port and CI-built example today; USB; abundant modules | Radio brings certification and security obligations; less common in regulated products |
| STM32 (family to be chosen) | Common in regulated and industrial products; wired only, no radio | No port or example yet; a family and a part must be chosen against the criteria |
| RP2040 or RP2350 | Low cost; good tooling | No port yet; integrators rarely ship it in regulated products |
| nRF | Low power, wireless | No port yet; radio obligations |

## Decision

Deferred. When N-009 is accepted, the first variant starts with the ESP32-S3 module because it is the one platform
whose driver example CI already builds, using a pre-certified module so the radio is not ours to certify, and with
the radio unused by the example. An STM32 variant follows once someone writes and CI-builds its driver port; the
family is chosen then, against these criteria, in a new ADR.

## Consequences

- Nothing is chosen for rev A. `mcu` stays `null`.
- A variant gets its own requirements, risks and verifications in this trace, and its own ADR for the part.
- Any statement about a regulated or medical use stays the integrator's to make.

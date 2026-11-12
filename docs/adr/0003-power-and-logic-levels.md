# ADR-0003: Power and logic levels

- Status: proposed
- Date: 2026-11-11
- Affects: N-004, R-003, R-008, R-009, R-010, H-001, H-002

## Context

The chip takes VDD from 1.71 V to 3.6 V and VDDIO from 1.08 V to 3.6 V, in any order (driver-bmp581
`docs/hardware.yml`, datasheet Table 4, p. 10). Development boards offer 3.3 V and 5 V rails, and some hosts use 1.8 V
logic. The base board must not need an adapter (N-004) and must never exceed the limits (R-003).

## Options considered

| Option | For | Against |
| --- | --- | --- |
| A. Pass-through: the user supplies VDD and VDDIO in range | Fewest parts; no heat near the sensor | A 5 V rail destroys the chip; users make that mistake |
| B. Onboard regulator from 5 V, with a 3.3 V direct path | Safe from either rail; one jumper to choose | A regulator warms the board; one more part and failure mode |
| C. B plus a level shifter for 5 V logic | Works with 5 V hosts completely | Two to three more parts, more area, a shifter on the I2C lines that the pull-ups and speed must suit |

## Decision

Option B. A solder jumper selects the 3.3 V direct path or the regulated path from the 5 V pin. VDDIO is tied to VDD
by a second solder jumper and can be cut and fed from its own pin for hosts below 3.3 V (R-009). No level shifter in
the base variant: the documentation and the silkscreen say that logic above VDDIO must not be connected, and what a
5 V host needs (R-010). The regulator sits away from the sensor (R-011).

## Consequences

- Regulator choice follows from R-008 and R-011: its output must stay within 3.6 V with margin across tolerance,
  and its quiescent current and dissipation must not warm the sensor. Selection happens in `kokeb-libs` with the
  review it requires.
- A 5 V logic host still needs an external level shifter. A later variant can add one (option C) if users ask.
- H-002 stays `open` until V-006 and V-007 give evidence.

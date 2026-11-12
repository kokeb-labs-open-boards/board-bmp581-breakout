# ADR-0002: Connectors and board size

- Status: proposed
- Date: 2026-11-11
- Affects: N-003, N-005, R-001, R-006, R-007, R-013

## Context

All eight signals of R-001 must be reachable (VDD, VDDIO, GND, SCK, SDI, SDO, CSB, INT). Users connect by breadboard,
by cable or on a fixture (N-003). The board is small and sits on a test rig (N-005).

## Options considered

| Option | For | Against |
| --- | --- | --- |
| A. One 2.54 mm single-row header | Fits breadboards and Dupont cables; every signal reachable; easy to inspect | Needs soldering of the header (supplied loose or fitted); no keyed plug |
| B. Qwiic / STEMMA QT connectors only | No soldering; polarised; chainable | I2C and 3.3 V only: no SPI, no separate VDDIO, no INT |
| C. Both | Covers the plug-in case and every signal | About 6 mm more length and two more parts |
| D. Castellated edge | Can be soldered onto a carrier | No breadboard use; harder to probe |

## Decision

Option C: a single-row 2.54 mm header with all eight signals, plus two Qwiic and STEMMA QT connectors wired in
parallel. A one-row header of eight pins is 20.32 mm long, so the provisional outline budget is 25.4 mm by 20.3 mm
(R-013). The layout may change that, with a recorded reason.

The header order puts the supplies at the ends and groups the bus signals, so that a reversed plug does not put a
supply on a signal pin as the first failure. The exact order is fixed at schematic review and documented (R-006).

## Consequences

- The Qwiic order is to be checked against the published specification before R-007 is approved.
- Two connectors in parallel mean the board is a pass-through on a chain; its address is fixed by the jumper
  (ADR-0004) and a chain needs distinct addresses.
- The outline, hole positions and port keep-out are confirmed by the first layout and then checked by V-008.
- Sourcing the connectors is a part-selection task in `kokeb-libs`, not decided here.

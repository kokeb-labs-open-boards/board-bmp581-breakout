# ADR-0005: Mechanical: mounting, the pressure port and enclosures

- Status: proposed
- Date: 2026-11-11
- Affects: N-005, R-011, R-012, R-013, H-003

## Context

A pressure sensor measures what reaches its port, so the mechanical design is part of the measurement. The base board
goes on a bench or a rig (N-005). Whether anyone wants an enclosure is not known yet.

## Options considered

| Option | For | Against |
| --- | --- | --- |
| A. Bare PCB with two mounting holes | Smallest, cheapest; nothing to block the port | No protection |
| B. Bare PCB plus a printable mount or case, as a separate mechanical variant | Protects and mounts; port vent designed in; source kept as code-CAD | Another design to verify; a case can block or trap air |
| C. Sealed enclosure | Protects against dust and splash | A sealed case traps air, so the sensor reads the case, not the room. Claims of ingress protection need real tests |

## Decision

Rev A is option A: two non-plated mounting holes for M2.5 screws (R-012), outline within 25.4 mm by 20.3 mm
(R-013). Two holes on a board this small hold it without stressing the sensor; the exact positions come from the
layout. A case is option B, added only after a need for it is recorded: N-005 is satisfied by the bare board.
Option C is not offered. No board or case from this repository claims an ingress rating.

## Consequences

- When a case is designed, its dimensions come from the board STEP that `kicad-cli` exports, never from
  measurement, and a vent over the port is a requirement with a verification (see `mechanical/README.md`).
- R-011 is verified by layout review and a thermal analysis, then confirmed on the bench (V-008).
- The datasheet package drawing and port position are cited when the footprint is reviewed in `kokeb-libs`.

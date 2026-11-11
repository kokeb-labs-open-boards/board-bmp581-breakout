# Hardware

The KiCad project for the board lives here, in `hardware/<board>/`: `<board>.kicad_pro`, `.kicad_sch` and
`.kicad_pcb`, with hierarchical sheets for each circuit block.

## Rules

- **KiCad 9 or newer.** Commit the text files KiCad writes; do not commit `*-backups/`, `*.kicad_prl`, `fp-info-cache`
  or `_autosave-*` (the `.gitignore` already skips them).
- **Parts come from `kokeb-libs`.** Use the symbols, footprints and 3D models that have passed review there, pinned
  by release tag. A part that is not there yet is added to `kokeb-libs` first, through its review, then used here.
- **Circuits come from `kokeb-blocks` where one exists.** A new circuit that could be reused becomes a block.
- **Chip facts come from the driver.** Pins, supply ranges and reference circuits follow the driver repository's
  `docs/hardware.yml`; a board that differs says why in an ADR.
- **Every schematic net has a reason.** Each requirement in `trace/requirements.yml` that the layout implements is
  named in the sheet it belongs to, in a text note: `Implements R-002`.
- **Reviews are files.** A schematic review and a layout review are written up in `hardware/reviews/` before the
  matching gate in [`docs/index.md`](../docs/index.md).

## Layout

- `bmp581-breakout/`: the KiCad project (an empty sheet and an empty board so far). Its library tables point at
  `${KOKEB_LIBS}`, the checkout of `kokeb-libs` at the pinned release tag.
- `reviews/`: written schematic and layout reviews, the evidence for the review gates.
- `../.kibot/release.kibot.yaml`: ERC, DRC and the release outputs (Gerbers, drill, BOM, placement, schematic PDF,
  STEP), run by the `Hardware` workflow and kept as a build artifact.

## Checks

The `Hardware` workflow runs ERC and DRC and generates the outputs whenever this folder changes. It is not a required
check until it has run green on a real design. The KiCad project files here were written by hand, not saved from
KiCad, and have not been opened in KiCad yet: open them once, save, and commit the result.

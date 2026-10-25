# Sketchpad Reconstruction

## Objective

Produce a complete plain-text TX-2 M4 assembly listing for Ivan
Sutherland's Sketchpad.  The listing must assemble with a documented
cross-assembler and reproduce the printed octal words where those words
survive.  A readable C translation is a later deliverable.

## Preserved Baselines

| Material | Upstream commit | Local marker |
| --- | --- | --- |
| TX-2/Sketchpad | `1263829ba8561d03b5c364bf7e339e5dd2d400ed` | `upstream-baseline-2026-09-14` |
| TX-2/TX-2-simulator | `8757845b8aeb1777c73ee2b9ad2885dd66e26288` | `sketchpad-reconstruction-baseline-2026-09-14` |
| CHM Historical Highlights Documents Repository | `31425aaad4268b6b225409b1e1b21c5c8076a0a6` | Pinned source URLs in `sources/README.md` |

The reconstruction branch starts at the unmodified Sketchpad baseline.
The simulator remains in a separate sibling checkout.  Changes to the
simulator must use their own branch and commits.

## Toolchain Baseline

- Host: macOS 15.7.3 on x86_64.
- Cargo: 1.98.1 (`797e8a9bc`, 2026-08-05).
- Rust: 1.98.1 (`48a229cea`, 2026-09-01).
- Simulator workspace build: successful.

Build the simulator:

```sh
cd TX-2-simulator
cargo build --workspace
```

Run the Sketchpad assembly check:

```sh
cd TX-2-simulator/sources/Sketchpad
./scripts/assemble.sh
```

## Unmodified Assembly Baseline

At the time Graham Goudeau began the reconstruction work, the
assembler failed at line 451 of sk.tx2as, but that problem with the
Sketchpad source was recently solved (in commit
119d62507ba18d63c043b86c253f985dce73599f), so now assembly fails for a
different reason, at line 493.

```text
error in user program: line 493, column 13: found '≡' expected ·, something else, symex syllable, arithmetic operator, '->', ',', instruction hold bit, program instruction, or '}'
```

The problem is not that the transscribed program is actually
incorrect, it is just that the assembler does not yet fully support
all the needed features.  bad transcription.

## Evidence Order

Use the strongest available evidence first:

1. Printed octal output beside the original source line.
2. A second occurrence in either surviving Sketchpad listing.
3. M4 syntax and TX-2 instruction encoding.
4. Local symbol, macro, control-flow, and data-flow constraints.
5. Behavior described in Sutherland's thesis and related primary records.
6. A semantic reconstruction with no direct textual witness.

## Change Classes

Every source change must have one class in the Git commit message and in
the reconstruction log.

- `mechanical`: The change preserves known source meaning and only changes
  representation or assembler compatibility.
- `verified`: Printed octal output or another primary copy proves the
  reconstructed text.
- `inferred`: Program constraints select one reading, but no direct copy
  proves it.
- `speculative`: More than one reading remains plausible.

Do not place speculative text in the canonical assembly file unless the
file marks it clearly and the log records the alternatives.

## Reconstruction Log

| ID | File and location | Class | Evidence | Result |
| --- | --- | --- | --- | --- |
| R000 | `sk.tx2as:451` | baseline | Assembler diagnostic and upstream limitation documentation (but this is out-of-date, as mentioned above) | Compound macro glyph blocks parsing. |

## Publication Gate

The upstream Sketchpad repository has no license file.  The Computer
History Museum also limits reuse of its scans.  Confirm publication rights
before a public fork or release.  Keep downloaded scans outside Git.

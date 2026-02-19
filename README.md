# TAP — Acoustic Profile for Stringed Instruments

An open standard for capturing the acoustic identity of stringed instruments, from raw materials to finished masterpiece.

## What is TAP?

A `.tap` file is a JSON document that records the acoustic and physical properties of a stringed instrument — what it's made of, how it sounds, and how it changes over time.

TAP is designed for luthiers, repair techs, researchers, and anyone who wants to understand and share the voice of a stringed instrument.

### Key Principles

- **Instrument-agnostic** — guitars, violins, mandolins, ouds, banjos, any stringed instrument
- **Material-agnostic** — wood, carbon fiber, composites, anything
- **Incrementally populated** — only two required fields (`tap_version` and `instrument.id`). Everything else is optional. A partial record is better than no record.
- **Append-friendly** — acoustic profiles are timestamped snapshots that capture how an instrument's voice evolves
- **Privacy-respecting** — built-in anonymization, licensing, and embargo controls

## Quick Start

A minimal valid TAP file:

```json
{
  "tap_version": "1.0",
  "instrument": {
    "id": "550e8400-e29b-41d4-a716-446655440000",
    "type": "acoustic-guitar",
    "name": "Build #14"
  }
}
```

A more complete example with materials and an acoustic profile: [examples/acoustic-guitar-om.tap](examples/acoustic-guitar-om.tap)

## Specification

📄 **[TAP-SPEC.md](TAP-SPEC.md)** — the full specification

## Examples

The [`examples/`](examples/) directory contains sample `.tap` files for common instrument types:

| File | Description |
|---|---|
| [`acoustic-guitar-om.tap`](examples/acoustic-guitar-om.tap) | OM acoustic guitar with materials and acoustic profile |
| [`classical-guitar-minimal.tap`](examples/classical-guitar-minimal.tap) | Minimal classical guitar — just instrument and materials |
| [`violin.tap`](examples/violin.tap) | Violin with acoustic profile |

## Contributing

TAP is an open standard and we welcome community input. See [CONTRIBUTING.md](CONTRIBUTING.md) for how to:

- Propose changes to the spec
- Submit example `.tap` files
- Discuss proposed extensions

## License

The TAP specification is licensed under [CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/). Implementations may use any license.

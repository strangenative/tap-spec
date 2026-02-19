# Contributing to TAP

Thanks for your interest in the Tonal Acoustic Profile standard. TAP is an open, community-driven specification and we welcome contributions at every level.

## Ways to Contribute

### Discuss the Spec

Open an [issue](../../issues) to:
- Ask questions about the spec
- Propose changes or additions
- Report ambiguities or inconsistencies
- Discuss proposed extensions (see the "Proposed Extensions" section of the spec)

### Submit Example Files

We welcome `.tap` files from real instruments. To contribute:

1. Fork this repo
2. Add your `.tap` file to `examples/`
3. Validate it (see below)
4. Submit a pull request

**Guidelines for examples:**
- Use a fresh UUID v4 for `instrument.id`
- Omit personal details from `builder` if you prefer to remain anonymous
- Include a brief description in `instrument.description`

### Propose Spec Changes

For changes to the spec itself:

1. Open an issue describing the change and rationale
2. Discuss with the community
3. If there's consensus, submit a PR against `TAP-SPEC.md`

## Validating TAP Files

A valid TAP file must:
- Be valid JSON
- Be UTF-8 encoded
- Include `tap_version` (string)
- Include `instrument.id` (string, UUID v4 recommended)

```bash
# Basic validation — check it's valid JSON with required fields
python3 -c "
import json, sys
d = json.load(open(sys.argv[1]))
assert 'tap_version' in d, 'missing tap_version'
assert 'instrument' in d and 'id' in d['instrument'], 'missing instrument.id'
print('✅ Valid TAP file')
" your-file.tap
```

A formal JSON Schema validator is planned for a future release.

## Code of Conduct

Be kind, be constructive, be respectful of builders at all skill levels. This community exists to help everyone make better instruments.

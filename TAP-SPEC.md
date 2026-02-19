# TAP — Acoustic Profile

### An open standard for capturing the acoustic identity of stringed instruments, from raw materials to finished masterpiece.

**Version:** 1.0-draft  
**Extension:** `.tap`  
**MIME type:** `application/x-tap+json`  
**License:** CC-BY 4.0 (the spec itself); implementations may use any license

---

## Overview

A TAP file is a JSON document that records the acoustic and physical properties of a stringed instrument — what it's made of, how it sounds, and how it changes over time.

TAP is designed to be:

- **Instrument-agnostic** — guitars, violins, mandolins, ouds, banjos, cellos, harps, kotas, any stringed instrument
- **Material-agnostic** — wood, carbon fiber, fiberglass, composites, gourds, skins, synthetics
- **Incrementally populated** — every field is optional except `instrument.id` and `tap_version`. A file with only wood measurements is valid. A file with only a finished acoustic profile is valid. Spotty records are expected and welcomed.
- **Append-friendly** — acoustic profiles are captured as timestamped snapshots, preserving the journey, not just the destination
- **Shareable** — simple JSON files that are easy to exchange, publish, and aggregate

---

## File Format

- **Encoding:** UTF-8
- **Structure:** A single JSON object conforming to this spec
- **Extension:** `.tap`

---

## Schema

### Root Object

```json
{
  "tap_version": "1.0",
  "instrument": { },
  "builder": { },
  "materials": [ ],
  "acoustic_profiles": [ ],
  "notes": [ ],
  "meta": { }
}
```

All top-level fields except `tap_version` and `instrument` are optional.

---

### `instrument`

Describes the instrument being built or profiled.

```json
{
  "id": "uuid-v4",
  "name": "The Oakland",
  "type": "acoustic-guitar",
  "family": "plucked",
  "body_shape": "OM",
  "scale_length_mm": 648,
  "string_config": {
    "count": 6,
    "courses": 6,
    "tuning": ["E2", "A2", "D3", "G3", "B3", "E4"],
    "fretted": true,
    "fret_count": 19
  },
  "build_date": "2026-02",
  "serial_number": "EB-014",
  "description": "Fan-braced classical guitar, Engelmann spruce top, Indian rosewood back and sides"
}
```

| Field | Type | Required | Notes |
|---|---|---|---|
| `id` | string (UUID v4) | **yes** | Globally unique identifier |
| `name` | string | no | Builder's name for this instrument |
| `type` | string | recommended | Open vocabulary. Common values: `acoustic-guitar`, `classical-guitar`, `archtop-guitar`, `electric-guitar`, `violin`, `viola`, `cello`, `double-bass`, `mandolin`, `mandola`, `mandocello`, `ukulele`, `banjo`, `oud`, `lute`, `bouzouki`, `sitar`, `erhu`, `kora`, `harp`, `dulcimer`, `charango`. Any string accepted. |
| `family` | string | no | Excitation family: `plucked`, `bowed`, `struck`, `mixed` |
| `body_shape` | string | no | Open vocabulary. Guitar examples: `dreadnought`, `OM`, `OOO`, `jumbo`, `parlor`, `classical`, `concert`. |
| `scale_length_mm` | number | no | Vibrating string length, nut to saddle. For multi-scale, use array. |
| `string_config` | object | no | See below |
| `build_date` | string | no | ISO 8601 date or partial date (`2026`, `2026-02`, `2026-02-19`) |
| `serial_number` | string | no | Builder's serial/catalog number |
| `description` | string | no | Free text |

#### `string_config`

| Field | Type | Notes |
|---|---|---|
| `count` | integer | Total individual strings |
| `courses` | integer | Number of courses (groups played together). Standard guitar: courses = count = 6. 12-string: count=12, courses=6. |
| `tuning` | string[] | One entry per course. Scientific pitch notation. Doubled courses use `+` separator (e.g. `"E2+E3"`). |
| `fretted` | boolean | `true` for fretted, `false` for fretless |
| `fret_count` | integer | Number of frets. Omit for fretless. |

---

### `builder`

```json
{
  "id": "uuid-v4",
  "name": "Russ Maschmeyer",
  "location": "Oakland, CA",
  "shop_name": "Maschmeyer Guitars",
  "website": "https://example.com"
}
```

All fields optional. The `id` enables linking builds by the same builder across a community database without requiring real names (when `anonymize_builder` is set in `meta`).

---

### `materials[]`

An array of material records. Each describes one physical component of the instrument.

```json
{
  "id": "mat-001",
  "component": "soundboard",
  "component_detail": "top plate, bookmatched",
  "material_class": "wood",
  "species": "engelmann-spruce",
  "species_detail": "Picea engelmannii, Colorado origin",
  "supplier": "LMI",
  "grade": "AAA",
  "acquired_date": "2025-11-15",

  "physical": {
    "dimensions_mm": { "length": 540, "width": 220, "thickness": 4.5 },
    "weight_g": 185,
    "density_kg_m3": 380,
    "grain_spacing_mm": 1.8,
    "moisture_content_pct": 7.5
  },

  "acoustic": {
    "tap_tones": [
      { "mode": "longitudinal", "frequency_hz": 680, "damping": 0.012 },
      { "mode": "cross", "frequency_hz": 195, "damping": 0.018 }
    ],
    "speed_of_sound_long_m_s": 5800,
    "speed_of_sound_cross_m_s": 1200,
    "radiation_ratio": 12.5
  },

  "photos": [
    { "path": "photos/soundboard-raw.jpg", "caption": "Grain pattern, bookmatched" }
  ],

  "notes": "Beautiful tight grain, slight bear claw figuring. Very stiff for its weight."
}
```

#### `component` vocabulary (open, these are common values)

**Universal:** `soundboard`, `back`, `sides`, `neck`, `fingerboard`, `bridge`, `saddle`, `nut`, `braces`, `linings`, `bindings`, `purfling`, `tailpiece`, `endpin`, `tuning-machines`

**Guitar-specific:** `bridge-plate`, `rosette`, `pickguard`, `truss-rod`

**Bowed-instrument-specific:** `bass-bar`, `sound-post`, `chinrest`, `f-hole-linings`

**Banjo-specific:** `head-membrane`, `tone-ring`, `resonator-back`, `flange`

Any string is accepted. Use `component_detail` for specifics.

#### `material_class` vocabulary

`wood`, `carbon-fiber`, `fiberglass`, `composite`, `laminate`, `metal`, `bone`, `ivory`, `synthetic`, `skin-membrane`, `gourd`, `bamboo`, `stone`, `resin`, `other`

#### `species` — for wood

Open vocabulary. Common: `sitka-spruce`, `engelmann-spruce`, `european-spruce`, `red-cedar`, `redwood`, `cypress`, `indian-rosewood`, `brazilian-rosewood`, `african-blackwood`, `ebony`, `maple`, `mahogany`, `walnut`, `koa`, `cocobolo`, `ziricote`, `santos-rosewood`, `australian-blackwood`, `paulownia`

For non-wood materials, use `material_detail` instead:

```json
{
  "material_class": "carbon-fiber",
  "material_detail": "Nomex honeycomb core with carbon fiber skins, 2mm total"
}
```

---

### `acoustic_profiles[]`

An array of acoustic snapshots taken at different points in the instrument's life. This is the heart of TAP — capturing how an instrument sounds and how that evolves over time.

```json
{
  "id": "uuid-v4",
  "profile_date": "2026-02-19",
  "profile_type": "full",

  "recording": {
    "device": "iPhone 16 Pro",
    "mic_position": "30cm from 12th fret, on-axis with soundhole",
    "environment": "Quiet workshop, no HVAC",
    "sample_rate_hz": 44100,
    "bit_depth": 16
  },

  "notes_analyzed": [
    {
      "course": 6,
      "position": 0,
      "expected_frequency_hz": 82.41,
      "detected_frequency_hz": 82.35,
      "fundamental_strength": 0.72,
      "harmonic_powers": [1.0, 0.68, 0.45, 0.31, 0.22, 0.15, 0.09, 0.06],
      "spectral_centroid_hz": 412,
      "sustain_t60_s": 4.2,
      "attack_time_ms": 12,
      "inharmonicity": 0.0003,
      "audio_file": "audio/6_0.wav"
    }
  ],

  "body_resonance": {
    "helmholtz_hz": 98,
    "main_top_hz": 192,
    "notes": "Measured by tap on bridge with strings damped"
  },

  "derived": {
    "sustain_evenness": 0.82,
    "brightness_gradient": 0.65,
    "wolf_notes": [
      { "course": 3, "position": 7, "type": "sustain-drop" }
    ]
  },

  "notes": "First profile, 24 hours after stringing. Strings still settling."
}
```

#### `position` field

For **fretted** instruments: fret number (0 = open string, 1 = first fret, etc.)

For **fretless** instruments: position in mm from the nut, or a named position. Use `position_type` to clarify:

```json
{ "course": 1, "position": 0, "position_type": "fret" }
{ "course": 1, "position": 335, "position_type": "mm-from-nut" }
{ "course": 1, "position": "1st", "position_type": "named-position" }
```

#### `profile_type`

- `full` — every string/position recorded
- `partial` — subset of positions (e.g., open strings only)
- `body-only` — tap response of body without string excitation

---

### `notes[]`

Free-form timestamped journal entries. The builder's narrative.

```json
[
  {
    "timestamp": "2026-02-10T10:00:00Z",
    "text": "Starting build #14. This Engelmann top has been in my shop for 6 months, well acclimated. Grain is tight and even, slight bear claw."
  },
  {
    "timestamp": "2026-02-16T14:30:00Z",
    "text": "Final voicing done. This top is more responsive than #12 was at this stage."
  }
]
```

---

### `meta`

```json
{
  "created": "2026-02-10T10:00:00Z",
  "modified": "2026-02-19T18:00:00Z",
  "software": {
    "name": "Tap",
    "version": "1.0.0",
    "platform": "macOS"
  },
  "custom": {}
}
```

#### `custom`

An open object for implementation-specific or community-specific extensions. Unknown fields elsewhere in the document should also be preserved (not stripped) by conforming software.

---

## Conformance

A **conforming TAP writer** must:
1. Include `tap_version` and `instrument.id`
2. Produce valid JSON
3. Use UTF-8 encoding
4. Preserve any fields it doesn't understand (no stripping unknown keys)

A **conforming TAP reader** must:
1. Accept any valid JSON with `tap_version` and `instrument.id`
2. Gracefully ignore unknown fields
3. Not require any field beyond the two mandatory ones
4. Support TAP versions it understands, and warn (not fail) on unknown versions

---

## Design Principles

1. **Every field optional (except two).** A partial record is better than no record.
2. **Physics-first, instrument-second.** Core measurements (frequency, stiffness, spectral analysis) are universal. Instrument-specific context is metadata.
3. **Snapshots, not state.** Profiles are immutable snapshots in time. The journey is as valuable as the destination.
4. **Human-readable.** JSON, meaningful field names, inline documentation via `notes` fields everywhere.
5. **Extensible.** Unknown fields are preserved. The `custom` field in `meta` is a sanctioned extension point. Future versions of the spec add fields but don't remove them.

---

## Proposed Extensions

The following extensions are under consideration for future versions of TAP. They are documented here to preserve intent and invite community discussion. Each could be adopted independently.

### Plate Tuning (`plate_tuning[]`)

Track the acoustic evolution of plates (soundboard, back) during the thinning process. An array of tuning records per plate, each containing a history of tap tone measurements at different thicknesses.

Key concepts:
- **Tuning history:** Timestamped snapshots of tap tone modes (0,0 / 1,0 / 0,1) at each thinning stage
- **Thickness maps:** Grid measurements of plate thickness at specific coordinates
- **Stiffness maps:** Relative stiffness at grid points
- **Voicing targets:** Target frequency ranges per mode, so the app can show progress toward a goal

Coordinate system: origin (0,0) at center of plate at the waist (guitar) or center of plate (violin). X = lateral, Y = longitudinal.

```json
{
  "component": "soundboard",
  "material_ref": "mat-001",
  "tuning_history": [
    {
      "timestamp": "2026-02-10T10:00:00Z",
      "stage": "initial",
      "avg_thickness_mm": 4.5,
      "tap_tones": [
        { "mode": "0_0", "frequency_hz": 320 },
        { "mode": "1_0", "frequency_hz": 145 }
      ]
    }
  ],
  "targets": {
    "mode_0_0_hz": { "min": 170, "max": 200 },
    "mode_1_0_hz": { "min": 90, "max": 110 }
  }
}
```

**Value:** High for serious builders who tune plates. This is where TAP data becomes most scientifically interesting — correlating plate modes with finished instrument tone across many builds.

**Complexity:** Significant. Requires coordinate systems, modal analysis vocabulary, and a more involved recording workflow.

---

### Bracing (`bracing`)

Document bracing patterns and track how each brace affects the acoustic response of the top.

Key concepts:
- **Brace inventory:** Individual brace records with position, dimensions, material, profile shape
- **Voicing history:** Tap tone snapshots after each brace is added or shaped, showing the cumulative effect
- **Voicing targets:** Target mode frequencies for the braced top
- **Pattern vocabulary:** Fan-7, X-braced, ladder, etc.

```json
{
  "component": "soundboard",
  "pattern": "fan-7",
  "braces": [
    {
      "id": "brace-01",
      "name": "waist bar",
      "species": "sitka-spruce",
      "dimensions_mm": { "length": 280, "height": 12, "width": 6 },
      "profile": "straight, tapered ends"
    }
  ],
  "voicing_history": [
    {
      "timestamp": "2026-02-14T15:00:00Z",
      "stage": "after-waist-bar",
      "braces_installed": ["brace-01"],
      "tap_tones": [
        { "mode": "0_0", "frequency_hz": 210 },
        { "mode": "1_0", "frequency_hz": 105 }
      ]
    }
  ]
}
```

**Value:** Very high for builders refining their craft. Seeing how individual braces shift modes across many builds could transform bracing from intuition to informed experimentation.

**Complexity:** Moderate. Depends on plate tuning extension for full value.

---

### Assembly (`assembly`)

Setup and construction details: neck angle, action, relief, compensation, string spacing, body depth, finish.

```json
{
  "neck_angle_degrees": 1.5,
  "action_mm": [
    { "course": 1, "at_fret": 12, "mm": 2.4 },
    { "course": 6, "at_fret": 12, "mm": 3.2 }
  ],
  "relief_mm": 0.15,
  "string_spacing_mm": { "at_nut": 43, "at_saddle": 57 },
  "finish": { "type": "french-polish", "material": "shellac" },
  "total_weight_g": 1650
}
```

**Value:** Moderate. Useful for correlating setup with playability and tone, but less acoustically unique than materials or profiles.

**Complexity:** Low. Flat key-value data, easy to collect.

---

### Strings (`strings`)

Brand, model, material, tension, gauges, install date.

```json
{
  "brand": "D'Addario",
  "model": "EJ46",
  "material": "nylon",
  "tension": "hard",
  "gauges_mm": [0.72, 0.83, 1.02, 0.74, 0.91, 1.09],
  "installed_date": "2026-02-18"
}
```

**Value:** Low-moderate. Strings are consumable and change frequently. Most useful when correlated with acoustic profiles (same strings = comparable profiles).

**Complexity:** Trivial.

---

### Binary Archive Format (`.tapx`)

A ZIP archive containing the TAP JSON plus associated audio files, photos, and other binary assets:

```
my-guitar.tapx (ZIP)
├── profile.tap
├── audio/
│   ├── E2_0.wav
│   └── ...
├── photos/
│   ├── soundboard-raw.jpg
│   └── ...
└── assets/
```

**Value:** Essential once tooling exists. Audio files are what make profiles verifiable and scientifically useful.

**Complexity:** Low for the format itself. Tooling cost is in the recording/management UX.

---

### Additional Future Considerations

- **Recording tiers & excitation standards:** Standardized recording quality levels and mechanical excitation methods for reproducible profiles
- **Computed fields registry:** Standardized formulas for derived values (radiation ratio, sonic rating, etc.) so implementations produce comparable numbers
- **Community database API:** REST spec for uploading, querying, and comparing TAP files
- **Instrument templates:** Pre-filled partial TAP files for common instrument types with typical target ranges
- **Chladni pattern images:** Standardized capture and linking of mode visualization photos
- **Sensor integration:** Real-time environmental data (humidity, temperature) linked to measurements

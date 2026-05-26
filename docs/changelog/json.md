---
title: Changelog — Clarotech.OpenEHR.RM.Json
description: Release history for the openEHR JSON serialisation library.
---

# Changelog — `Clarotech.OpenEHR.RM.Json`

Source: [clarotech/openEHR-json](https://github.com/clarotech/openEHR-json)

---

## 1.0.0

> Released 2026-05-25. First stable release.

### Serialisation

- `OpenEhrJsonSerializer.Serialize<T>()` — compact or pretty-printed Canonical JSON
- `OpenEhrJsonSerializer.SerializeByRuntimeType()` — runtime-type dispatch, correct `_type` on abstract-declared variables
- `OpenEhrJsonOptions.Default` — shared, pre-configured `JsonSerializerOptions`

### Deserialisation

- `OpenEhrJsonSerializer.Deserialize<T>(string)` — from string
- `OpenEhrJsonSerializer.Deserialize<T>(Stream)` — from synchronous stream
- `OpenEhrJsonSerializer.DeserializeAsync<T>(Stream)` — async stream deserialisation

### Format compliance

- openEHR Canonical JSON `_type` discriminator written first on every object
- `snake_case` property names (`JsonNamingPolicy.SnakeCaseLower`)
- Null-valued properties omitted (`WhenWritingNull`)
- Case-insensitive deserialisation

### Coverage

Full converter coverage for:

| RM package | Types |
|---|---|
| Data Types | All `DvXxx` types including generic `DvInterval<T>` and `ReferenceRange<T>` |
| Common | `PartyProxy` hierarchy, `AuditDetails`, `Attestation`, `Link`, `FeederAudit`, `RevisionHistory` |
| Data Structures | `ItemTree/List/Table/Single`, `Cluster`, `Element`, `History<T>`, `PointEvent<T>`, `IntervalEvent<T>` |
| Composition | `Composition`, `Section`, all four entry types + `AdminEntry`, `EventContext`, `Archetyped` |
| Change Control | `OriginalVersion<T>`, `ImportedVersion<T>`, `Contribution` |
| EHR | `Ehr`, `EhrStatus`, `EhrAccess`, `VersionedComposition`, `VersionedEhrStatus`, `VersionedEhrAccess` |
| Support | `UidBasedId`, `ObjectId`, `ObjectRef` |

### Limitations

- ⚠️ Reflection-based; not safe with `PublishTrimmed=true` or Native AOT

---

*For the authoritative change log see the
[GitHub Releases page](https://github.com/clarotech/openEHR-json/releases).*

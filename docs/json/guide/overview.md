---
title: JSON — Introduction
description: Architecture and design of the Clarotech.OpenEHR.RM.Json serialisation library.
---

# JSON Serialisation — Introduction

## openEHR Canonical JSON

The [openEHR REST API Specification](https://specifications.openehr.org/releases/ITS-REST/latest)
defines a **Canonical JSON** encoding for all RM objects. `Clarotech.OpenEHR.RM.Json`
implements this encoding faithfully. The key rules are:

- **`_type` discriminator** — every serialised RM object carries a `"_type"` field with the
  openEHR RM type name (e.g. `"DV_QUANTITY"`, `"COMPOSITION"`, `"OBSERVATION"`).
  This is written as the **first** property of each JSON object so consumers can resolve the
  concrete type before reading remaining fields.
- **`snake_case` property names** — all property names are `snake_case` (e.g. `archetype_node_id`,
  `defining_code`). This is enforced via `JsonNamingPolicy.SnakeCaseLower`.
- **Null omission** — properties with `null` values are omitted entirely from the output
  (`DefaultIgnoreCondition = WhenWritingNull`).
- **Case-insensitive deserialisation** — the deserialiser accepts both `snake_case` and
  `camelCase` property names, making it tolerant of minor format variations from third-party
  openEHR CDRs.

## The two public types

### `OpenEhrJsonSerializer`

The static entry point for all serialisation and deserialisation. Three method groups:

| Method | Description |
|---|---|
| `Serialize<T>(T, bool)` | Serialise using the **declared** type `T` |
| `SerializeByRuntimeType(object, bool)` | Serialise using the **runtime** type — use this when the declared type is abstract (e.g. `DataValue`) to ensure the correct `_type` is emitted |
| `Deserialize<T>(string)` | Deserialise from a JSON string |
| `Deserialize<T>(Stream)` | Deserialise from a UTF-8 byte stream |
| `DeserializeAsync<T>(Stream, CancellationToken)` | Async stream deserialisation |

### `OpenEhrJsonOptions`

```csharp
JsonSerializerOptions opts = OpenEhrJsonOptions.Default;
```

`OpenEhrJsonOptions.Default` is a thread-safe, shared `JsonSerializerOptions` instance
pre-loaded with converters for every type in the RM hierarchy. You can layer additional
converters on top:

```csharp
var custom = new JsonSerializerOptions(OpenEhrJsonOptions.Default);
custom.Converters.Add(new MyCustomConverter());
string json = JsonSerializer.Serialize(value, custom);
```

## Converter architecture

Converters are organised to mirror the RM package structure:

```
Converters/
├── DataValueConverter.cs        — polymorphic root; reads "_type" to dispatch
├── DataTypes/                   — DvText, DvCodedText, DvQuantity, DvCount,
│                                  DvProportion, DvOrdinal, DvBoolean,
│                                  DvDateTime/Date/Time/Duration,
│                                  DvIdentifier, DvUri, DvEhrUri,
│                                  DvMultimedia, DvParsable
├── Common/                      — PartyProxy (→ Identified/Self/Related),
│                                  AuditDetails, Attestation, Link,
│                                  FeederAudit, RevisionHistory
├── DataStructures/              — ItemStructure, History<T>, Event<T>,
│                                  ItemTree/List/Table/Single, Cluster, Element
├── Composition/                 — Composition, Section, ContentItem,
│                                  Observation, Evaluation, Instruction,
│                                  Action, AdminEntry, EventContext
├── ChangeControl/               — Version<T>, OriginalVersion<T>,
│                                  ImportedVersion<T>, Contribution
├── Ehr/                         — Ehr, EhrStatus, EhrAccess,
│                                  VersionedComposition, VersionedEhrStatus
└── Support/                     — UidBasedId, ObjectId, ObjectRef
```

## Trim / AOT note

All converters use reflection and are annotated with `[RequiresUnreferencedCode]`.
The library is **not safe** for use with `PublishTrimmed=true` or Native AOT publishing
without a custom source-generated serialiser context. If you need AOT support, open an
issue on [clarotech/openEHR-json](https://github.com/clarotech/openEHR-json).

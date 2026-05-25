---
title: JSON Serialisation
description: Overview of the Clarotech.OpenEHR.RM.Json library.
---

# JSON Serialisation — `Clarotech.OpenEHR.RM.Json`

[![NuGet](https://img.shields.io/nuget/v/Clarotech.OpenEHR.RM.Json)](https://www.nuget.org/packages/Clarotech.OpenEHR.RM.Json)
[![GitHub](https://img.shields.io/badge/source-clarotech%2FopenEHR--json-blue?logo=github)](https://github.com/clarotech/openEHR-json)

`Clarotech.OpenEHR.RM.Json` provides **canonical JSON serialisation and deserialisation**
for `Clarotech.OpenEHR.RM` and `Clarotech.OpenEHR.RM.Datatypes` using `System.Text.Json`.

## Key characteristics

| Feature | Detail |
|---|---|
| **JSON format** | [openEHR Canonical JSON](https://specifications.openehr.org/releases/ITS-REST/latest) — `snake_case` property names, `_type` discriminator on every RM object |
| **Null handling** | Null-valued properties are omitted from output |
| **Deserialisation** | Case-insensitive property name matching |
| **Entry point** | `OpenEhrJsonSerializer` — three static methods |
| **Options** | `OpenEhrJsonOptions.Default` — pre-configured `JsonSerializerOptions` |
| **AOT / trimming** | ⚠️ Reflection-based; not trim-safe (annotated with `[RequiresUnreferencedCode]`) |

## Installation

```bash
dotnet add package Clarotech.OpenEHR.RM.Json
# Clarotech.OpenEHR.RM and Clarotech.OpenEHR.RM.Datatypes come in automatically
```

## Quick example

```csharp
using Clarotech.OpenEHR.RM.Json;

// Serialise — picks up _type from the runtime type of composition
string json = OpenEhrJsonSerializer.Serialize(composition, writeIndented: true);

// Deserialise
var restored = OpenEhrJsonSerializer.Deserialize<Composition>(json);
```

## Namespace layout

| Namespace | Contents |
|---|---|
| `Clarotech.OpenEHR.RM.Json` | `OpenEhrJsonSerializer`, `OpenEhrJsonOptions` |
| `Clarotech.OpenEHR.RM.Json.Converters` | `DataValueConverter`, `ConverterHelpers` |
| `Clarotech.OpenEHR.RM.Json.Converters.DataTypes` | Per-type converters for all `DvXxx` classes |
| `Clarotech.OpenEHR.RM.Json.Converters.Common` | Party, audit, link, feeder-audit converters |
| `Clarotech.OpenEHR.RM.Json.Converters.DataStructures` | `ItemTree/List/Table/Single`, `Cluster`, `Element`, `History`, `Event` converters |
| `Clarotech.OpenEHR.RM.Json.Converters.Composition` | `Composition`, `Section`, entry-type converters |
| `Clarotech.OpenEHR.RM.Json.Converters.ChangeControl` | `Version<T>`, `Contribution` converters |
| `Clarotech.OpenEHR.RM.Json.Converters.Ehr` | `Ehr`, `EhrStatus`, `EhrAccess`, versioned-object converters |
| `Clarotech.OpenEHR.RM.Json.Converters.Support` | `UidBasedId`, `ObjectId`, `ObjectRef` converters |

## Guide sections

- [Introduction](guide/overview.md)
- [Serialisation](guide/serialization.md)
- [Deserialisation](guide/deserialization.md)

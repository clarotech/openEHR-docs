---
title: EHR RM — Introduction
description: Architecture and design patterns for the OpenEHR.RM namespace.
---

# EHR Reference Model — Introduction

`Clarotech.OpenEHR.RM` (`OpenEHR.RM.*`) implements the structural backbone of an openEHR
record store. It depends on `Clarotech.OpenEHR.RM.Datatypes` for all leaf data values.

## Design patterns

### `Locatable` — the identity backbone

Almost every class in the RM inherits from `Locatable`, which provides:

| Property | Type | Purpose |
|---|---|---|
| `Name` | `DvText` | Human-readable name (mandatory) |
| `ArchetypeNodeId` | `string` | `at0001`-style node ID from the constraining archetype |
| `Uid` | `UidBasedId?` | Optional local unique ID |
| `ArchetypeDetails` | `Archetyped?` | Archetype + template binding |
| `Links` | `ISet<Link>?` | Cross-references to other objects |

### `Pathable` — AQL path navigation

`Pathable` (base of `Locatable`) allows objects to be addressed using AQL-style paths
such as `/data[at0001]/items[at0002]/value`.

### `init`-only properties

All RM classes use C# `init`-only properties. Construct with object-initialiser syntax
and treat instances as effectively immutable after construction:

```csharp
var section = new Section
{
    Name           = new DvText("History"),
    ArchetypeNodeId = "at0001",
    Items          = new List<ContentItem> { /* entries */ }
};
```

## Package layout

| Package | Guide page |
|---|---|
| EHR, versioning, contribution | [EHR & Versioning](ehr.md) |
| Composition, sections, entries | [Composition](composition.md) |
| Common — audit, party, links | [Common](common.md) |
| Data structures — tree, list | [Data Structures](data-structures.md) |

---
title: EHR Reference Model
description: Overview of the Clarotech.OpenEHR.RM library.
---

# EHR Reference Model — `Clarotech.OpenEHR.RM`

[![NuGet](https://img.shields.io/nuget/v/Clarotech.OpenEHR.RM)](https://www.nuget.org/packages/Clarotech.OpenEHR.RM)
[![GitHub](https://img.shields.io/badge/source-clarotech%2FopenEHR--ehr-blue?logo=github)](https://github.com/clarotech/openEHR-ehr)

A C# implementation of the **openEHR RM Release 1.1.0** structural classes, covering
the EHR, Composition, Common, Support, and Data Structures packages.

> **Depends on** `Clarotech.OpenEHR.RM.Datatypes` — all `DvXxx` leaf values come
> from that package.

## Namespaces

| Namespace | openEHR package | Key classes |
|---|---|---|
| `OpenEHR.RM.Ehr` | EHR | `Ehr`, `EhrStatus`, `EhrAccess`, `Folder` |
| `OpenEHR.RM.Composition` | Composition | `Composition`, `Section`, `EventContext` |
| `OpenEHR.RM.Composition.Content.Entry` | Entry | `Observation`, `Evaluation`, `Instruction`, `Action`, `AdminEntry`, `GenericEntry` |
| `OpenEHR.RM.Composition.Content.Navigation` | Navigation | `Section` |
| `OpenEHR.RM.Common` | Common | `Archetyped`, `Link`, `Locatable`, `Pathable` |
| `OpenEHR.RM.Common.Archetyped` | Archetyped | `Archetyped`, `TemplateId`, `Locatable` |
| `OpenEHR.RM.Common.Generic` | Generic | `PartyIdentified`, `PartyRelated`, `PartySelf`, `AuditDetails`, `Attestation` |
| `OpenEHR.RM.Common.ChangeControl` | Change Control | `Contribution`, `Version<T>`, `OriginalVersion<T>`, `ImportedVersion<T>` |
| `OpenEHR.RM.DataStructures` | Data Structures | `ItemTree`, `ItemList`, `ItemTable`, `ItemSingle`, `Cluster`, `Element` |
| `OpenEHR.RM.Support.Identification` | Identification | `HierObjectId`, `ObjectVersionId`, `TemplateId`, `ArchetypeId`, `TerminologyId` |

## Installation

```bash
dotnet add package Clarotech.OpenEHR.RM
# Clarotech.OpenEHR.RM.Datatypes is pulled in automatically as a dependency
```

## Structural overview

```
Ehr
├── EhrStatus
├── EhrAccess
└── Composition (via Contribution/Version<T>)
    ├── EventContext
    ├── Section
    │   └── Entry (Observation | Evaluation | Instruction | Action)
    │       └── ItemStructure (ItemTree | ItemList | …)
    │           └── Element
    │               └── DataValue  (any DvXxx from datatypes)
    └── …
```

## Guide sections

- [Introduction](guide/overview.md)
- [EHR & Versioning](guide/ehr.md)
- [Composition](guide/composition.md)
- [Common (Audit, Link, Party)](guide/common.md)
- [Data Structures](guide/data-structures.md)

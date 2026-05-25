---
title: Overview
description: High-level overview of the Clarotech openEHR C# libraries.
---

# Overview

The Clarotech openEHR libraries provide a faithful, idiomatic C# implementation of the
[openEHR Reference Model (RM) Release 1.1.0](https://specifications.openehr.org/releases/RM/Release-1.1.0).
They are designed to make it straightforward to build EHR systems, clinical data pipelines, and
interoperability adapters in .NET without having to hand-roll the complex openEHR type hierarchy.

## What is openEHR?

openEHR is an open standard for representing, storing, and querying electronic health record (EHR)
data. Its **Reference Model** defines a small set of stable structural building blocks (compositions,
entries, sections, elements) and a rich set of **data types** (text, quantities, coded terms,
date/time values, …) on top of which clinical knowledge artefacts — **archetypes** and **templates** —
are layered.

## The three libraries at a glance

```
┌─────────────────────────────────────────────────────────────────────┐
│  Clarotech.OpenEHR.RM  (openEHR-ehr)                                │
│  Composition · Section · Entry · Element · Ehr · Folder · …         │
│                                                                     │
│  Depends on ▼                                                       │
│                                                                     │
│  Clarotech.OpenEHR.RM.Datatypes  (openEHR-datatypes)               │
│  DvText · DvCodedText · DvQuantity · DvDateTime · DvUri · …         │
│                                                                     │
│  Serialised by ▼                                                    │
│                                                                     │
│  Clarotech.OpenEHR.RM.Json  (openEHR-json)                          │
│  JSON serialisation / deserialisation for both libraries above       │
└─────────────────────────────────────────────────────────────────────┘
```

| Library | NuGet package | Root namespace |
|---|---|---|
| openEHR-datatypes | `Clarotech.OpenEHR.RM.Datatypes` | `OpenEHR.RM.DataTypes` |
| openEHR-ehr | `Clarotech.OpenEHR.RM` | `OpenEHR.RM` |
| openEHR-json | `Clarotech.OpenEHR.RM.Json` | *(TBD)* |

## Design principles

- **Spec-faithful** — class names, property names, and multiplicity constraints follow the openEHR
  RM specification directly, so the code is readable alongside the standard.
- **Immutability by default** — all RM classes use C# `init`-only properties; instances are
  constructed once and not mutated.
- **Lazy validation** — RM invariant checks are exposed through `IValidatableObject.Validate()`
  rather than throwing at construction time, giving consumers control over when validation runs.
- **Modern .NET** — targets `net8.0` and `net10.0`; leverages nullable reference types, pattern
  matching, and record semantics where appropriate.
- **Apache 2.0 licence** — permissive for both open-source and commercial use.

## Next steps

- [Getting Started](getting-started.md) — install the packages and write your first `Composition`.
- [openEHR Primer](openehr-primer.md) — a short crash course in openEHR concepts for .NET developers.

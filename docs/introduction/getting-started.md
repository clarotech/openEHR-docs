---
title: Getting Started
description: Install the Clarotech openEHR NuGet packages and build your first Composition.
---

# Getting Started

## Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0) or later
- A .NET 8 / .NET 10 class library or console project

## 1 — Install the packages

```bash
# Core data types (DvText, DvQuantity, DvDateTime, …)
dotnet add package Clarotech.OpenEHR.RM.Datatypes

# Structural classes (Composition, Entry, Section, …)
dotnet add package Clarotech.OpenEHR.RM

# JSON serialisation (optional — needed for persistence/transport)
dotnet add package Clarotech.OpenEHR.RM.Json
```

## 2 — Reference the namespaces

```csharp
using OpenEHR.RM;                        // Composition, Entry, Section, …
using OpenEHR.RM.DataTypes.Text;         // DvText, DvCodedText, TerminologyId, …
using OpenEHR.RM.DataTypes.Quantity;     // DvQuantity, DvCount, DvProportion, …
using OpenEHR.RM.DataTypes.DateTime;     // DvDateTime, DvDuration, …
using OpenEHR.RM.Support.Identification; // HierObjectId, ObjectVersionId, …
```

## 3 — Build a minimal Composition

The following snippet creates a skeleton `Composition` containing one
`Observation` entry with a single `DvQuantity` element.

```csharp
// TODO: expand this example once the library API is finalised
var composition = new Composition
{
    Uid     = new ObjectVersionId("a1b2c3d4-…::example.clarotech.com::1"),
    Name    = new DvText("Vital Signs"),
    Language = new CodePhrase(new TerminologyId("ISO_639-1"), "en"),
    // … additional required properties
};
```

> **Tip** — All RM classes use C# `init`-only properties, so the object-initialiser
> syntax above is the standard construction pattern.

## 4 — Validate

```csharp
using System.ComponentModel.DataAnnotations;

var results = new List<ValidationResult>();
bool valid  = Validator.TryValidateObject(composition, 
                  new ValidationContext(composition), results, validateAllProperties: true);

if (!valid)
    foreach (var r in results)
        Console.WriteLine(r.ErrorMessage);
```

## 5 — Serialise to JSON

```csharp
// TODO: JSON serialisation examples (Clarotech.OpenEHR.RM.Json)
```

→ See [Round-trip JSON](../integration/round-trip-json.md) for a complete serialise/deserialise example.

## Next steps

- [Data Types guide](../datatypes/index.md) — detailed coverage of every `Dv*` type.
- [EHR Reference Model guide](../ehr/index.md) — structural classes and their relationships.
- [Integration Guides](../integration/index.md) — end-to-end worked examples.

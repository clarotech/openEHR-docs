---
title: Basic Data Types
description: DvBoolean, DvState, and DvIdentifier.
---

# Basic Data Types

Namespace: `OpenEHR.RM.DataTypes.Basic`

The **Basic** package contains the root abstract class and three concrete leaf types that
represent fundamental non-specialist values.

## `DataValue` (abstract)

All openEHR data-type classes inherit from `DataValue`. It carries no mandatory
properties of its own but establishes the type hierarchy that allows any leaf value to
be stored in an `Element.Value`.

## `DvBoolean`

A simple true/false value. Maps to `DV_BOOLEAN` in the spec.

```csharp
var flag = new DvBoolean { Value = true };
```

> **Invariant** — `Value` must be set; the type has no meaningful null state.

## `DvState`

Represents a state machine state. Contains a `DvCodedText` (the state name) and an
optional boolean indicating whether the state is a terminal state.

```csharp
var state = new DvState
{
    Value      = new DvCodedText { Value = "active", DefiningCode = … },
    IsTerminal = false
};
```

> **Note** — `DvState` is rarely used in modern openEHR clinical archetypes; prefer
> `DvCodedText` for status values.

## `DvIdentifier`

An identifier from an external system (e.g. a hospital MRN, an NHS number).

```csharp
var mrn = new DvIdentifier
{
    Issuer   = "NHS Digital",
    Assigner = "NHS Digital",
    Id       = "943-476-0165",
    Type     = "NHS number"
};
```

| Property | Mandatory | Description |
|---|---|---|
| `Issuer`   | ✅ | Name of the organisation that issued the ID |
| `Assigner` | ✅ | Name of the organisation that assigned it |
| `Id`       | ✅ | The identifier string |
| `Type`     | ❌ | Optional classification (e.g. "MRN", "NHSNumber") |

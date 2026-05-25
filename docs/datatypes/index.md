---
title: Data Types
description: Overview of the Clarotech.OpenEHR.RM.Datatypes library.
---

# Data Types — `Clarotech.OpenEHR.RM.Datatypes`

[![NuGet](https://img.shields.io/nuget/v/Clarotech.OpenEHR.RM.Datatypes)](https://www.nuget.org/packages/Clarotech.OpenEHR.RM.Datatypes)
[![GitHub](https://img.shields.io/badge/source-clarotech%2FopenEHR--datatypes-blue?logo=github)](https://github.com/clarotech/openEHR-datatypes)

A C# implementation of the **openEHR RM Release 1.1.0 Data Types** specification
(`DATA_TYPES` package).  All leaf values in an openEHR record are instances of
a class from this library.

## Namespaces

| Namespace | Contents |
|---|---|
| `OpenEHR.RM.DataTypes.Basic` | `DataValue` (abstract base), `DvBoolean`, `DvState`, `DvIdentifier` |
| `OpenEHR.RM.DataTypes.Text` | `DvText`, `DvCodedText`, `CodePhrase`, `TerminologyId` |
| `OpenEHR.RM.DataTypes.Quantity` | `DvOrdered<T>`, `DvQuantifiable<T>`, `DvQuantity`, `DvCount`, `DvProportion`, `DvOrdinal`, `DvAmount`, `DvMeasurable` |
| `OpenEHR.RM.DataTypes.DateTime` | `DvTemporal<T>`, `DvDate`, `DvTime`, `DvDateTime`, `DvDuration` |
| `OpenEHR.RM.DataTypes.TimeSpecification` | `DvTimeSpecification`, `DvPeriodicTime`, `DvGeneralTimeSpecification` |
| `OpenEHR.RM.DataTypes.Encapsulated` | `DvEncapsulated`, `DvMultimedia`, `DvParsable` |
| `OpenEHR.RM.DataTypes.Uri` | `DvUri`, `DvEhrUri` |

## Installation

```bash
dotnet add package Clarotech.OpenEHR.RM.Datatypes
```

## Quick example

```csharp
using OpenEHR.RM.DataTypes.Text;
using OpenEHR.RM.DataTypes.Quantity;

// A plain text value
var note = new DvText("Patient reports mild headache.");

// A coded term (SNOMED CT)
var diagnosis = new DvCodedText
{
    Value         = "Headache",
    DefiningCode  = new CodePhrase(new TerminologyId("SNOMED-CT"), "25064002")
};

// A physical quantity with units
var heartRate = new DvQuantity { Magnitude = 72, Units = "/min" };
```

## Guide sections

- [Introduction](guide/overview.md)
- [Basic Data Types](guide/basic.md)
- [Text Types](guide/text.md)
- [Quantity Types](guide/quantity.md)
- [Date & Time Types](guide/datetime.md)
- [Encapsulated Data](guide/encapsulated.md)
- [URI Types](guide/uri.md)

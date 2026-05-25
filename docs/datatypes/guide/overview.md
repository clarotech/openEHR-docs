---
title: Data Types — Introduction
description: Architecture and usage patterns for the OpenEHR.RM.DataTypes namespace.
---

# Data Types — Introduction

The `OpenEHR.RM.DataTypes` namespace hierarchy maps directly to the openEHR RM
[Data Types Information Model](https://specifications.openehr.org/releases/RM/Release-1.1.0/data_types.html).
Every class name, property name, and multiplicity constraint is taken from the specification,
making the code readable alongside the standard.

## Inheritance hierarchy

```
DataValue  (abstract)
├── DvBoolean
├── DvState
├── DvIdentifier
├── DvText
│   └── DvCodedText
├── DvEncapsulated
│   ├── DvMultimedia
│   └── DvParsable
├── DvUri
│   └── DvEhrUri
├── DvTimeSpecification
│   ├── DvPeriodicTime
│   └── DvGeneralTimeSpecification
└── DvOrdered<T>
    ├── DvOrdinal
    └── DvQuantifiable<T>
        ├── DvAmount<T>
        │   ├── DvQuantity          (has units, e.g. "mg/dL")
        │   ├── DvCount             (dimensionless integer)
        │   ├── DvDuration          (ISO 8601 period)
        │   └── DvProportion        (ratio / percentage)
        └── DvTemporal<T>
            ├── DvDate
            ├── DvTime
            └── DvDateTime
```

## Construction pattern

All data-type classes use C# `init`-only properties:

```csharp
var qty = new DvQuantity
{
    Magnitude         = 98.6,
    Units             = "[degF]",
    Precision         = 1,
    NormalStatus      = new CodePhrase(new TerminologyId("openehr"), "N"),
    MagnitudeStatus   = "="
};
```

## Validation

```csharp
using System.ComponentModel.DataAnnotations;

var ctx     = new ValidationContext(qty);
var results = new List<ValidationResult>();
bool ok     = Validator.TryValidateObject(qty, ctx, results, true);
```

## Next

Pick a specific type family to dive into:

- [Basic Data Types](basic.md)
- [Text Types](text.md)
- [Quantity Types](quantity.md)
- [Date & Time Types](datetime.md)
- [Encapsulated Data](encapsulated.md)
- [URI Types](uri.md)

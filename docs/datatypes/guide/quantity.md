---
title: Quantity Types
description: DvQuantity, DvCount, DvProportion, DvOrdinal, and the ordered/quantifiable hierarchy.
---

# Quantity Types

Namespace: `OpenEHR.RM.DataTypes.Quantity`

## Ordered hierarchy

```
DvOrdered<T>
├── DvOrdinal            — ranked items (e.g. pain scale 0–10)
└── DvQuantifiable<T>
    ├── DvAmount<T>
    │   ├── DvQuantity   — physical measurement with UCUM units
    │   ├── DvCount      — dimensionless integer count
    │   ├── DvDuration   — ISO 8601 duration (P1Y2M3DT4H…)
    │   └── DvProportion — ratio / fraction / percentage
    └── DvTemporal<T>    — (see Date & Time guide)
```

## `DvQuantity`

The workhorse type for physical measurements. Units are expressed using
[UCUM](https://ucum.org/) notation.

```csharp
// Heart rate
var hr = new DvQuantity { Magnitude = 72,   Units = "/min" };

// Temperature (Fahrenheit)
var temp = new DvQuantity { Magnitude = 98.6, Units = "[degF]", Precision = 1 };

// Blood pressure systolic
var bp = new DvQuantity { Magnitude = 120, Units = "mm[Hg]" };
```

Key properties:

| Property | Type | Description |
|---|---|---|
| `Magnitude` | `double` | The numeric value |
| `Units` | `string` | UCUM unit code |
| `Precision` | `int?` | Decimal places; `-1` = unconstrained |
| `MagnitudeStatus` | `string?` | `"="` exact, `"<"` less than, `">"` greater than, `"~"` approx |
| `NormalStatus` | `CodePhrase?` | `"N"` normal, `"H"` high, `"L"` low, … |
| `NormalRange` | `DvInterval<DvQuantity>?` | Reference range |

## `DvCount`

A dimensionless integer quantity (e.g. number of episodes, parity).

```csharp
var parity = new DvCount { Magnitude = 2 };
```

## `DvProportion`

Represents ratios, percentages, fractions, and unitary values.

```csharp
// 75 %
var saturation = new DvProportion
{
    Numerator   = 75,
    Denominator = 100,
    Type        = ProportionKind.Percent    // 2 = percent
};

// Ratio 1:4
var ratio = new DvProportion
{
    Numerator   = 1,
    Denominator = 4,
    Type        = ProportionKind.Ratio      // 0 = ratio
};
```

`ProportionKind` values: `Ratio(0)`, `Unitary(1)`, `Percent(2)`, `Fraction(3)`, `IntegerFraction(4)`.

## `DvOrdinal`

A ranked item from a discrete ordered set. Stores both a numeric value and a symbol.

```csharp
var painScore = new DvOrdinal
{
    Value  = 7,
    Symbol = new DvCodedText { Value = "Severe", DefiningCode = … }
};
```

> **Ordering** — `DvOrdinal` implements `IComparable` using the numeric `Value`.

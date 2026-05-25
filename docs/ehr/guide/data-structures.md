---
title: Data Structures
description: ItemTree, ItemList, ItemTable, ItemSingle, Cluster, and Element.
---

# Data Structures

Namespace: `OpenEHR.RM.DataStructures`

openEHR uses a small set of generic container classes to carry an entry's clinical content.
The choice between them is constrained by archetypes, not by the RM itself.

## Structure types

| Class | openEHR equivalent | Shape |
|---|---|---|
| `ItemTree` | `ITEM_TREE` | Recursive tree of named items — most common |
| `ItemList` | `ITEM_LIST` | Flat ordered list of `Element` (no nesting) |
| `ItemTable` | `ITEM_TABLE` | Table of rows, each a `Cluster` |
| `ItemSingle` | `ITEM_SINGLE` | Exactly one `Element` |

## `ItemTree`

The workhorse structure. Holds a flat or nested mix of `Element` and `Cluster` items.

```csharp
var tree = new ItemTree
{
    Name  = new DvText("Tree"),
    Items = new List<Item>
    {
        new Element
        {
            Name            = new DvText("Systolic"),
            ArchetypeNodeId = "at0004",
            Value           = new DvQuantity { Magnitude = 120, Units = "mm[Hg]" }
        },
        new Element
        {
            Name            = new DvText("Diastolic"),
            ArchetypeNodeId = "at0005",
            Value           = new DvQuantity { Magnitude = 80, Units = "mm[Hg]" }
        }
    }
};
```

## `Cluster`

A named group of `Item` objects within an `ItemTree`, allowing nested structure.

```csharp
var bpCluster = new Cluster
{
    Name            = new DvText("Blood pressure reading"),
    ArchetypeNodeId = "at0006",
    Items           = new List<Item> { systolicElement, diastolicElement }
};
```

## `Element`

The atomic leaf node. Each `Element` holds exactly one `DataValue` in its `Value` property.

```csharp
var ageElement = new Element
{
    Name            = new DvText("Age"),
    ArchetypeNodeId = "at0007",
    Value           = new DvCount { Magnitude = 45 }
};
```

| Property | Type | Description |
|---|---|---|
| `Value` | `DataValue?` | The clinical value (any `DvXxx` type) |
| `NullFlavour` | `DvCodedText?` | Reason value is absent (e.g. "unknown", "not asked") |
| `NullReason` | `DvText?` | Free-text reason for null value |

### Null flavours

When `Value` is `null`, `NullFlavour` must be set using the openEHR null-flavour vocabulary
(`terminology_id = "openehr"`):

| Code | Meaning |
|---|---|
| 253 | unknown |
| 271 | not applicable |
| 272 | masked |
| 273 | not asked |

## `ItemList`

A flat ordered list — useful for simple observation panels where no nesting is needed.

```csharp
var medications = new ItemList
{
    Name  = new DvText("Medication list"),
    Items = new List<Element> { drugElement, doseElement, routeElement }
};
```

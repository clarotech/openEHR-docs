---
title: Integration Guides
description: End-to-end worked examples using all three Clarotech openEHR libraries together.
---

# Integration Guides

These guides show how the three libraries work together to accomplish real-world tasks.

| Guide | What it demonstrates |
|---|---|
| [Building a Composition](building-a-composition.md) | Assembling a complete Vital Signs composition from scratch using all RM layers |
| [Round-Trip JSON](round-trip-json.md) | Serialise a Composition to Canonical JSON and deserialise it back |

## Prerequisites

All guides assume you have installed all three packages:

```bash
dotnet add package Clarotech.OpenEHR.RM.Datatypes
dotnet add package Clarotech.OpenEHR.RM
dotnet add package Clarotech.OpenEHR.RM.Json   # once published
```

And the top-level using directives:

```csharp
using OpenEHR.RM;
using OpenEHR.RM.Composition;
using OpenEHR.RM.Composition.Content.Entry;
using OpenEHR.RM.DataStructures;
using OpenEHR.RM.DataTypes.Text;
using OpenEHR.RM.DataTypes.Quantity;
using OpenEHR.RM.DataTypes.DateTime;
using OpenEHR.RM.Common.Generic;
using OpenEHR.RM.Support.Identification;
```

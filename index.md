---
_layout: landing
---

# Clarotech openEHR Libraries

> **C# implementations of the [openEHR Reference Model Release 1.1.0](https://specifications.openehr.org/releases/RM/Release-1.1.0)** —
> strongly-typed, NuGet-ready, targeting .NET 8 and .NET 10.

---

## Libraries

| Package | NuGet | Description |
|---|---|---|
| [`Clarotech.OpenEHR.RM.Datatypes`](docs/datatypes/index.md) | [![NuGet](https://img.shields.io/nuget/v/Clarotech.OpenEHR.RM.Datatypes)](https://www.nuget.org/packages/Clarotech.OpenEHR.RM.Datatypes) | openEHR RM data types — `DvText`, `DvQuantity`, `DvDateTime`, `DvCodedText`, … |
| [`Clarotech.OpenEHR.RM`](docs/ehr/index.md) | [![NuGet](https://img.shields.io/nuget/v/Clarotech.OpenEHR.RM)](https://www.nuget.org/packages/Clarotech.OpenEHR.RM) | openEHR RM structural classes — `Composition`, `Entry`, `Section`, `Element`, … |
| [`Clarotech.OpenEHR.RM.Json`](docs/json/index.md) | *(coming soon)* | JSON serialisation/deserialisation for the above two libraries |

---

## Quick start

```bash
dotnet add package Clarotech.OpenEHR.RM.Datatypes
dotnet add package Clarotech.OpenEHR.RM
```

```csharp
using OpenEHR.RM.Composition;
using OpenEHR.RM.DataTypes.Text;

var text = new DvText("Chief complaint: chest pain");
```

→ See [Getting Started](docs/introduction/getting-started.md) for a complete walkthrough.

---

## About

These libraries are developed and maintained by **[Claro Consulting](https://clarotech.github.io)** and are published under the **Apache 2.0** licence.

The openEHR Reference Model is a freely available open standard published by the [openEHR Foundation](https://www.openehr.org/).

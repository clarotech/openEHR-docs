---
title: URI Types
description: DvUri and DvEhrUri for linking to external resources and within an EHR.
---

# URI Types

Namespace: `OpenEHR.RM.DataTypes.Uri`

## `DvUri`

A simple wrapper around a URI string. Used anywhere a hyperlink to an external resource
is needed within an openEHR record.

```csharp
var link = new DvUri("https://pubmed.ncbi.nlm.nih.gov/12345678/");
var localFile = new DvUri("file:///data/obs/ecg-2025-05-24.pdf");
```

> **Invariant** — `Value` must be a syntactically valid URI (RFC 3986).

## `DvEhrUri`

A specialised URI using the `ehr://` scheme for intra-EHR links — for example to link
an action back to its originating instruction, or to cross-reference another composition
within the same EHR.

```csharp
// Link to a specific version of a composition
var link = new DvEhrUri(
    "ehr://a1b2c3d4-e5f6-…/compositions/7f8g9h0i-…::hospital.example.com::1");
```

The `ehr://` URI format is:

```
ehr://<ehr-id>/<path>
```

where `<path>` follows the AQL path syntax used in openEHR queries.

## `LINK` (Related type in openEHR-ehr)

While `DvUri` and `DvEhrUri` carry plain URI values, the `LINK` class in the
`OpenEHR.RM` package (in the openEHR-ehr library) wraps a `DvEhrUri` with a typed
relationship and a human-readable meaning:

```csharp
// (in OpenEHR.RM namespace)
var followUp = new Link
{
    Meaning = new DvText("follow up"),
    Type    = new DvText("issue"),
    Target  = new DvEhrUri("ehr://…")
};
```

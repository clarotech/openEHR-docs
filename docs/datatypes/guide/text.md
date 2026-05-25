---
title: Text Types
description: DvText, DvCodedText, CodePhrase, and TerminologyId.
---

# Text Types

Namespace: `OpenEHR.RM.DataTypes.Text`

## `DvText`

The simplest text data type. Represents a human-readable string that **may** carry
optional formatting, language, and encoding metadata.

```csharp
var note = new DvText("Patient reports mild chest pain.");

// With optional metadata
var richNote = new DvText
{
    Value    = "Patient reports <b>mild</b> chest pain.",
    Encoding = new CodePhrase(new TerminologyId("IANA_character-sets"), "UTF-8"),
    Language = new CodePhrase(new TerminologyId("ISO_639-1"), "en"),
    Formatting = "text/html"
};
```

> **Invariant** — `Value` must be non-empty.

## `DvCodedText`

`DvCodedText` extends `DvText` by adding a `DefiningCode` — a reference into a
terminology system. This is the most commonly used type for coded clinical concepts.

```csharp
var snomedDiagnosis = new DvCodedText
{
    Value        = "Hypertensive disorder",
    DefiningCode = new CodePhrase(
                       new TerminologyId("SNOMED-CT"),
                       "38341003")
};

var loincObservation = new DvCodedText
{
    Value        = "Blood pressure panel",
    DefiningCode = new CodePhrase(
                       new TerminologyId("LOINC"),
                       "55284-4")
};
```

## `CodePhrase`

A `(terminology_id, code_string)` pair. Terminology IDs use openEHR-defined names:

| openEHR terminology ID | External standard |
|---|---|
| `"SNOMED-CT"` | SNOMED Clinical Terms |
| `"LOINC"` | Logical Observation Identifiers Names and Codes |
| `"ICD10"` | ICD-10 diagnosis codes |
| `"ISO_639-1"` | Language codes (`"en"`, `"de"`, …) |
| `"IANA_character-sets"` | Character-set names (`"UTF-8"`, …) |
| `"openehr"` | openEHR internal vocabulary (lifecycle states, category codes, …) |

```csharp
var lang = new CodePhrase(new TerminologyId("ISO_639-1"), "en");
```

## `TerminologyId`

A wrapper around the terminology name string, with an optional version:

```csharp
var tid = new TerminologyId("SNOMED-CT");       // no version
var tidV = new TerminologyId("SNOMED-CT(2024)"); // with version
```

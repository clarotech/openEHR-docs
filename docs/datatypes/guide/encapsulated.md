---
title: Encapsulated Data
description: DvMultimedia and DvParsable for binary and structured text payloads.
---

# Encapsulated Data

Namespace: `OpenEHR.RM.DataTypes.Encapsulated`

Encapsulated types carry payloads that are not directly interpretable by the RM itself —
either binary content (`DvMultimedia`) or structured text in an external format (`DvParsable`).

## `DvEncapsulated` (abstract)

The abstract base for both types. Carries language, charset, and optional size/hash metadata.

| Property | Type | Description |
|---|---|---|
| `Charset` | `CodePhrase?` | IANA character set (e.g. `"UTF-8"`) |
| `Language` | `CodePhrase?` | ISO 639-1 language code |

## `DvMultimedia`

Stores binary content such as DICOM images, PDFs, audio, or video — either inline as a
`byte[]` or by reference via a `DvUri`.

```csharp
// Inline PNG
var ecgImage = new DvMultimedia
{
    MediaType    = new CodePhrase(new TerminologyId("IANA_media-types"), "image/png"),
    Data         = File.ReadAllBytes("ecg.png"),
    Size         = new FileInfo("ecg.png").Length,
    IntegrityCheck = ComputeSha256("ecg.png"),
    IntegrityCheckAlgorithm = new CodePhrase(
                                  new TerminologyId("openehr_integrity_check_algorithms"),
                                  "SHA-256")
};

// By reference (the binary lives elsewhere)
var ctScan = new DvMultimedia
{
    MediaType = new CodePhrase(new TerminologyId("IANA_media-types"), "application/dicom"),
    Uri       = new DvUri("https://pacs.hospital.example/studies/1.2.3.4")
};
```

> **Guidance** — Prefer `Uri` references for large binaries to avoid inflating composition
> sizes. Store the DICOM in a PACS and reference it.

## `DvParsable`

Stores structured text content whose internal format is known but is not interpreted by
the openEHR RM — for example an HL7 v2 message segment, an HTML fragment, or a CDA
document excerpt.

```csharp
var hl7Segment = new DvParsable
{
    Value       = "OBX|1|NM|8867-4^Heart rate^LN||72|/min|…",
    Formalism   = "hl7v2_segment"
};

var htmlNote = new DvParsable
{
    Value     = "<p>Follow-up in <strong>4 weeks</strong></p>",
    Formalism = "text/html"
};
```

The `Formalism` property is a free-form string identifying the syntax of the payload.

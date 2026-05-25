---
title: Common — Audit, Link, Party
description: AuditDetails, party types, Link, and Archetyped.
---

# Common — Audit, Link, Party

Namespace: `OpenEHR.RM.Common`, `OpenEHR.RM.Common.Generic`, `OpenEHR.RM.Common.Archetyped`

## Audit

### `AuditDetails`

Records who changed something, when, from what system, and why.

```csharp
var audit = new AuditDetails
{
    SystemId      = "hospital.example.com",
    Committer     = new PartyIdentified
    {
        Name        = "Dr Alice Smith",
        Identifiers = new List<DvIdentifier>
        {
            new DvIdentifier { Issuer = "GMC", Id = "1234567" }
        }
    },
    TimeCommitted = new DvDateTime(DateTimeOffset.UtcNow.ToString("o")),
    ChangeType    = new DvCodedText
    {
        Value        = "creation",
        DefiningCode = new CodePhrase(new TerminologyId("openehr"), "249")
    },
    Description   = new DvText("Initial vital signs recording.")
};
```

openEHR change type codes (`terminology_id = "openehr"`):

| Code | Meaning |
|---|---|
| 249 | creation |
| 250 | amendment |
| 251 | modification |
| 252 | synthesis |
| 523 | unknown |
| 666 | deleted |

### `Attestation`

An `Attestation` records a formal sign-off (e.g. a clinician verifying a result):

```csharp
var attestation = new Attestation
{
    Attester    = new PartyIdentified { Name = "Dr Bob Jones" },
    TimeAttested = new DvDateTime(…),
    Reason      = new DvText("Verified result"),
    IsPending   = false
};
```

## Party types

| Class | Usage |
|---|---|
| `PartyIdentified` | Named party with optional external identifiers |
| `PartyRelated` | Party with a relationship to the subject (e.g. "next of kin") |
| `PartySelf` | The patient themselves (used as `subject` on entries) |

```csharp
// Clinician with GMC number
var gp = new PartyIdentified
{
    Name        = "Dr Alice Smith",
    Identifiers = new List<DvIdentifier>
    {
        new DvIdentifier { Issuer = "GMC", Id = "1234567", Type = "GMC" }
    },
    ExternalRef = new PartyRef(
        new GenericId("https://ods.nhs.uk/practitioner/G1234567", "internet"),
        "DEMOGRAPHIC", "PERSON")
};

// Patient (self)
var subject = new PartySelf();
```

## `Link`

Creates a typed relationship between two `Locatable` objects:

```csharp
var link = new Link
{
    Meaning = new DvText("result of"),
    Type    = new DvText("issue"),
    Target  = new DvEhrUri("ehr://patient-ehr-id/compositions/instruction-version-id")
};
```

## `Archetyped`

Binds a `Locatable` to an archetype and optionally a template:

```csharp
var archetyped = new Archetyped
{
    ArchetypeId = new ArchetypeId("openEHR-EHR-OBSERVATION.pulse.v2"),
    TemplateId  = new TemplateId("Vital Signs Encounter"),
    RmVersion   = "1.1.0"
};
```

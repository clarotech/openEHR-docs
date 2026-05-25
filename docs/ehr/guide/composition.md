---
title: Composition
description: The Composition, Section, and Entry classes.
---

# Composition

Namespace: `OpenEHR.RM.Composition`, `OpenEHR.RM.Composition.Content.Entry`

A **Composition** is the top-level clinical document in openEHR — the unit of clinical
meaning that a clinician creates, signs, and submits.

## `Composition`

```csharp
var composition = new Composition
{
    Name            = new DvText("Vital Signs"),
    ArchetypeNodeId = "openEHR-EHR-COMPOSITION.encounter.v1",
    ArchetypeDetails = new Archetyped
    {
        ArchetypeId = new ArchetypeId("openEHR-EHR-COMPOSITION.encounter.v1"),
        RmVersion   = "1.1.0"
    },
    Language  = new CodePhrase(new TerminologyId("ISO_639-1"), "en"),
    Territory = new CodePhrase(new TerminologyId("ISO_3166-1"), "GB"),
    Category  = new DvCodedText
    {
        Value        = "event",
        DefiningCode = new CodePhrase(new TerminologyId("openehr"), "433")
    },
    Composer = new PartyIdentified { Name = "Dr Alice Smith" },
    Context  = new EventContext
    {
        StartTime = new DvDateTime(DateTimeOffset.UtcNow.ToString("o")),
        Setting   = new DvCodedText { Value = "primary medical care", DefiningCode = … }
    },
    Content = new List<ContentItem> { myObservation }
};
```

Key mandatory properties:

| Property | Description |
|---|---|
| `Language` | ISO 639-1 language code for the composition content |
| `Territory` | ISO 3166-1 country where the composition was created |
| `Category` | `"event"` (point-in-time) or `"persistent"` (ongoing) |
| `Composer` | The person or system authoring the composition |
| `Context` | Clinical context (care setting, timestamps, participants) |
| `Content` | Ordered list of `ContentItem` (Section or Entry) |

## `Section`

A grouping container within a composition, equivalent to a heading in a clinical document.

```csharp
var historySection = new Section
{
    Name           = new DvText("History"),
    ArchetypeNodeId = "at0001",
    Items          = new List<ContentItem> { chiefComplaintObservation }
};
```

## Entries

Four archetyped entry types represent the four kinds of clinical statement:

| Class | openEHR code | Usage |
|---|---|---|
| `Observation` | `OBSERVATION` | Measurements, findings |
| `Evaluation` | `EVALUATION` | Assessments, diagnoses, goals |
| `Instruction` | `INSTRUCTION` | Orders, prescriptions |
| `Action` | `ACTION` | Performed procedures, dispensed medications |

### `Observation` example

```csharp
var hrObservation = new Observation
{
    Name             = new DvText("Heart rate"),
    ArchetypeNodeId  = "openEHR-EHR-OBSERVATION.pulse.v2",
    Language         = new CodePhrase(new TerminologyId("ISO_639-1"), "en"),
    Encoding         = new CodePhrase(new TerminologyId("IANA_character-sets"), "UTF-8"),
    Subject          = new PartySelf(),
    Data = new History<ItemStructure>
    {
        Name   = new DvText("History"),
        Events = new List<Event<ItemStructure>>
        {
            new PointEvent<ItemStructure>
            {
                Name = new DvText("Any event"),
                Time = new DvDateTime(DateTimeOffset.UtcNow.ToString("o")),
                Data = new ItemTree
                {
                    Name  = new DvText("Tree"),
                    Items = new List<Item>
                    {
                        new Element
                        {
                            Name            = new DvText("Rate"),
                            ArchetypeNodeId = "at0004",
                            Value           = new DvQuantity { Magnitude = 72, Units = "/min" }
                        }
                    }
                }
            }
        }
    }
};
```

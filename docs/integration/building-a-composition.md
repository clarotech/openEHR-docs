---
title: Building a Composition
description: Step-by-step guide to assembling a complete Vital Signs composition.
---

# Building a Composition

This guide walks through building a complete **Vital Signs** `Composition` containing
blood pressure and heart rate `Observation` entries, then wrapping it in a `Version<T>`
and `Contribution`.

## 1 — Data type values

```csharp
// Blood pressure readings
var systolic  = new DvQuantity { Magnitude = 120, Units = "mm[Hg]", Precision = 0 };
var diastolic = new DvQuantity { Magnitude = 80,  Units = "mm[Hg]", Precision = 0 };

// Heart rate
var heartRate = new DvQuantity { Magnitude = 72, Units = "/min", Precision = 0 };

// Recording time
var now = new DvDateTime(DateTimeOffset.UtcNow.ToString("o"));
```

## 2 — Element / ItemTree

```csharp
var bpTree = new ItemTree
{
    Name            = new DvText("Tree"),
    ArchetypeNodeId = "at0001",
    Items = new List<Item>
    {
        new Element
        {
            Name            = new DvText("Systolic"),
            ArchetypeNodeId = "at0004",
            Value           = systolic
        },
        new Element
        {
            Name            = new DvText("Diastolic"),
            ArchetypeNodeId = "at0005",
            Value           = diastolic
        }
    }
};

var hrTree = new ItemTree
{
    Name            = new DvText("Tree"),
    ArchetypeNodeId = "at0001",
    Items = new List<Item>
    {
        new Element
        {
            Name            = new DvText("Rate"),
            ArchetypeNodeId = "at0004",
            Value           = heartRate
        }
    }
};
```

## 3 — Observations

```csharp
var bpObservation = new Observation
{
    Name             = new DvText("Blood Pressure"),
    ArchetypeNodeId  = "openEHR-EHR-OBSERVATION.blood_pressure.v2",
    Language         = new CodePhrase(new TerminologyId("ISO_639-1"), "en"),
    Encoding         = new CodePhrase(new TerminologyId("IANA_character-sets"), "UTF-8"),
    Subject          = new PartySelf(),
    Data = new History<ItemStructure>
    {
        Name            = new DvText("History"),
        ArchetypeNodeId = "at0001",
        Origin          = now,
        Events = new List<Event<ItemStructure>>
        {
            new PointEvent<ItemStructure>
            {
                Name            = new DvText("Any event"),
                ArchetypeNodeId = "at0006",
                Time            = now,
                Data            = bpTree
            }
        }
    }
};

var hrObservation = new Observation
{
    Name             = new DvText("Heart Rate"),
    ArchetypeNodeId  = "openEHR-EHR-OBSERVATION.pulse.v2",
    Language         = new CodePhrase(new TerminologyId("ISO_639-1"), "en"),
    Encoding         = new CodePhrase(new TerminologyId("IANA_character-sets"), "UTF-8"),
    Subject          = new PartySelf(),
    Data = new History<ItemStructure>
    {
        Name            = new DvText("History"),
        ArchetypeNodeId = "at0001",
        Origin          = now,
        Events = new List<Event<ItemStructure>>
        {
            new PointEvent<ItemStructure>
            {
                Name            = new DvText("Any event"),
                ArchetypeNodeId = "at0003",
                Time            = now,
                Data            = hrTree
            }
        }
    }
};
```

## 4 — Composition

```csharp
var composition = new Composition
{
    Name             = new DvText("Vital Signs"),
    ArchetypeNodeId  = "openEHR-EHR-COMPOSITION.encounter.v1",
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
        StartTime = now,
        Setting   = new DvCodedText
        {
            Value        = "primary medical care",
            DefiningCode = new CodePhrase(new TerminologyId("openehr"), "228")
        }
    },
    Content = new List<ContentItem> { bpObservation, hrObservation }
};
```

## 5 — Version and Contribution

```csharp
var compositionId = Guid.NewGuid().ToString();
var systemId      = "hospital.example.com";

var audit = new AuditDetails
{
    SystemId      = systemId,
    Committer     = new PartyIdentified { Name = "Dr Alice Smith" },
    TimeCommitted = now,
    ChangeType    = new DvCodedText
    {
        Value        = "creation",
        DefiningCode = new CodePhrase(new TerminologyId("openehr"), "249")
    }
};

var version = new OriginalVersion<Composition>
{
    Uid            = new ObjectVersionId($"{compositionId}::{systemId}::1"),
    Data           = composition,
    LifecycleState = new DvCodedText
    {
        Value        = "complete",
        DefiningCode = new CodePhrase(new TerminologyId("openehr"), "532")
    },
    CommitAudit    = audit
};
```

## 6 — Validate

```csharp
var ctx     = new ValidationContext(composition);
var results = new List<ValidationResult>();
bool ok     = Validator.TryValidateObject(composition, ctx, results, true);
Console.WriteLine(ok ? "Valid" : $"Invalid: {string.Join(", ", results.Select(r => r.ErrorMessage))}");
```

→ Next: [Round-Trip JSON](round-trip-json.md)

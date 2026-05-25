---
title: EHR & Versioning
description: The Ehr, EhrStatus, Contribution, and Version<T> classes.
---

# EHR & Versioning

Namespace: `OpenEHR.RM.Ehr`, `OpenEHR.RM.Common.ChangeControl`

## `Ehr`

The top-level container representing a single patient's electronic health record.
Each `Ehr` has a globally unique `EhrId` (`HierObjectId`).

```csharp
var ehr = new Ehr
{
    SystemId    = new HierObjectId("hospital.example.com"),
    EhrId       = new HierObjectId(Guid.NewGuid().ToString()),
    TimeCreated = new DvDateTime(DateTimeOffset.UtcNow.ToString("o")),
    EhrStatus   = new EhrStatus { … },
};
```

## `EhrStatus`

Carries demographic linkage, consent flags, and queryability settings for an EHR:

| Property | Description |
|---|---|
| `Subject` | `PartySelf` linking to the patient |
| `IsQueryable` | Whether the record appears in population queries |
| `IsModifiable` | Whether the record can be updated |

## Versioning

openEHR uses a first-class versioning model. Every mutable object (Composition, EhrStatus,
Folder) is wrapped in a `Version<T>` and grouped into a `Contribution`.

### `Contribution`

Equivalent to a git commit — an atomic change-set that groups one or more `Version<T>` objects:

```csharp
var contribution = new Contribution
{
    Uid     = new HierObjectId(Guid.NewGuid().ToString()),
    Versions = new HashSet<ObjectRef> { /* refs to the versions */ },
    Audit    = new AuditDetails
    {
        SystemId     = "hospital.example.com",
        Committer    = new PartyIdentified { Name = "Dr Smith" },
        TimeCommitted = new DvDateTime(DateTimeOffset.UtcNow.ToString("o")),
        ChangeType   = new DvCodedText { Value = "creation", DefiningCode = … }
    }
};
```

### `OriginalVersion<T>`

The first or updated version of an object:

```csharp
var version = new OriginalVersion<Composition>
{
    Uid             = new ObjectVersionId($"{guid}::hospital.example.com::1"),
    Data            = myComposition,
    LifecycleState  = new DvCodedText { Value = "complete", DefiningCode = … },
    CommitAudit     = contribution.Audit,
    Contribution    = new ObjectRef(contribution.Uid, "LOCAL", "CONTRIBUTION")
};
```

### `ObjectVersionId`

Version IDs take the form `<object_id>::<creating_system>::<version_tree_id>`:

```
"a1b2c3d4-e5f6-7890-abcd-ef1234567890::hospital.example.com::1"
```

Subsequent versions increment the tree ID: `::2`, `::3`, etc.
Branched versions use a `.` separator: `::1.1.1`.

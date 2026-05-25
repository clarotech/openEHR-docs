---
title: openEHR Primer
description: A short crash course in openEHR concepts for .NET developers.
---

# openEHR Primer

This page gives .NET developers enough openEHR background to use these libraries productively.
For the full specification see the [openEHR Foundation website](https://www.openehr.org/).

## The Information Hierarchy

```
EHR
└── EhrStatus          (one per EHR — demographic info, consent flags)
└── EhrAccess          (access-control metadata)
└── Contribution       (versioned change-set, like a git commit)
    └── Version<T>
        └── Composition      ← the main clinical document
            ├── Section      (structural grouping, like a folder)
            │   └── Entry    (the atomic clinical statement)
            │       └── Element  (a single data point, e.g. a measurement)
            └── …
```

A **Composition** is the top-level clinical document — roughly equivalent to a
clinical note, a discharge summary, or a vital-signs observation set.

## Entries

Entries are the four archetypable clinical statement types:

| Type | Class | Used for |
|---|---|---|
| Observation | `Observation` | Measurements, findings (e.g. blood pressure) |
| Evaluation | `Evaluation` | Assessments, diagnoses, risk evaluations |
| Instruction | `Instruction` | Orders, prescriptions, care plans |
| Action | `Action` | Performed procedures, medication administrations |

Each entry contains a **data** `ItemTree` or `ItemList` of `Element` items, each of which
holds a single `DvXxx` data-type value.

## Data Types

All leaf data in openEHR flows through the `DATA_VALUE` hierarchy:

| openEHR type | C# class | Example value |
|---|---|---|
| `DV_TEXT` | `DvText` | `"Chief complaint"` |
| `DV_CODED_TEXT` | `DvCodedText` | SNOMED CT 22253000 (pain) |
| `DV_QUANTITY` | `DvQuantity` | 98.6 °F |
| `DV_COUNT` | `DvCount` | 3 episodes |
| `DV_PROPORTION` | `DvProportion` | 75 % |
| `DV_DATE_TIME` | `DvDateTime` | 2025-05-24T09:00:00+01:00 |
| `DV_DURATION` | `DvDuration` | P1Y2M (1 year, 2 months) |
| `DV_URI` | `DvUri` | `https://example.com/obs/123` |
| `DV_EHR_URI` | `DvEhrUri` | `ehr://…` |
| `DV_MULTIMEDIA` | `DvMultimedia` | PDF, DICOM, PNG |
| `DV_PARSABLE` | `DvParsable` | HL7 v2 segment, CDA fragment |

## Terminology and Coded Text

A `DvCodedText` pairs a human-readable string with a `CodePhrase`:

```
DvCodedText
  ├── value       "Chest pain"
  └── defining_code
        CodePhrase
          ├── terminology_id  TerminologyId("SNOMED-CT")
          └── code_string     "29857009"
```

The terminology identifier uses openEHR-defined names: `"SNOMED-CT"`, `"LOINC"`,
`"ICD10"`, `"ISO_639-1"` (language codes), `"openehr"` (RM-internal codes), etc.

## Versioning

openEHR uses `OBJECT_VERSION_ID` strings of the form:
```
<object_id>::<creating_system_id>::<version_tree_id>
```
e.g. `"a1b2c3d4-…::hospital.example.com::1"`.

The `Version<T>` wrapper carries lifecycle metadata (lifecycle_state, commit_audit, contribution)
around any versionable object (Composition, EhrStatus, Folder).

## Archetypes and Templates (out of scope)

These libraries implement the **Reference Model** layer only. Archetype-level constraints
(openEHR Archetype Definition Language / ADL 2, operational templates / OPT2, and AQL querying)
are handled by separate tooling and are not part of these NuGet packages.

## Further reading

- [openEHR RM specification](https://specifications.openehr.org/releases/RM/Release-1.1.0)
- [openEHR Architecture Overview](https://specifications.openehr.org/releases/BASE/latest/architecture_overview.html)
- [Archetype Designer](https://tools.openehr.org/designer/) — browse existing archetypes

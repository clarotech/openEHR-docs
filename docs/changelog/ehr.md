---
title: Changelog — Clarotech.OpenEHR.RM
description: Release history for the openEHR EHR Reference Model library.
---

# Changelog — `Clarotech.OpenEHR.RM`

Source: [clarotech/openEHR-ehr](https://github.com/clarotech/openEHR-ehr)

---

## 1.0.0

> Aligns with openEHR RM Release 1.1.0

### Added

- Complete implementation of the openEHR EHR, Composition, Common, Support, and
  Data Structures packages
- `Observation`, `Evaluation`, `Instruction`, `Action`, `AdminEntry`, `GenericEntry` entry types
- `History<T>`, `PointEvent<T>`, `IntervalEvent<T>` — temporal event containers
- `ItemTree`, `ItemList`, `ItemTable`, `ItemSingle`, `Cluster`, `Element` — data structure types
- `Ehr`, `EhrStatus`, `EhrAccess`, `Folder`
- `Contribution`, `OriginalVersion<T>`, `ImportedVersion<T>`
- Full `AuditDetails`, `Attestation`, party types
- `IValidatableObject` on `Composition` and all `Locatable` subclasses
- Depends on `Clarotech.OpenEHR.RM.Datatypes` ≥ 1.1.0

---

*For the authoritative change log see the
[GitHub Releases page](https://github.com/clarotech/openEHR-ehr/releases).*

---
title: Changelog — Clarotech.OpenEHR.RM.Datatypes
description: Release history for the openEHR RM data types library.
---

# Changelog — `Clarotech.OpenEHR.RM.Datatypes`

Source: [clarotech/openEHR-datatypes](https://github.com/clarotech/openEHR-datatypes)

---

## 1.1.0

> Aligns with openEHR RM Release 1.1.0

### Added

- Full implementation of all seven `DATA_TYPES` sub-packages:
  `Basic`, `Text`, `Quantity`, `DateTime`, `TimeSpecification`, `Encapsulated`, `Uri`
- `IValidatableObject` support on all concrete `DataValue` classes for lazy RM invariant checking
- Nullable reference types enabled throughout
- `net10.0` target alongside `net8.0`

### Notes

- Package ID changed from `OpenEHR.RM.DataTypes` to `Clarotech.OpenEHR.RM.Datatypes` — update
  your `PackageReference` elements accordingly.

---

## 1.0.x

> Initial pre-release series — not recommended for production.

---

*For the authoritative change log see the
[GitHub Releases page](https://github.com/clarotech/openEHR-datatypes/releases).*

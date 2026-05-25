---
title: Date & Time Types
description: DvDate, DvTime, DvDateTime, DvDuration, and partial/complete ISO 8601 handling.
---

# Date & Time Types

Namespace: `OpenEHR.RM.DataTypes.DateTime`

All temporal types derive from `DvTemporal<T>` (which itself derives from `DvQuantifiable<T>`)
and are based on **ISO 8601**. A key openEHR feature is support for *partial* values — for
example a date of birth where only the year is known.

## `DvDate`

An ISO 8601 calendar date, optionally partial.

```csharp
// Complete date
var dob = new DvDate("1985-03-22");

// Year and month only (day unknown)
var approx = new DvDate("1985-03");

// Year only
var yearOnly = new DvDate("1985");
```

Partial precision is indicated by the length of the value string.

## `DvTime`

An ISO 8601 time of day, optionally partial, optionally timezone-aware.

```csharp
var appointmentTime = new DvTime("09:30:00+01:00");
var hourOnly        = new DvTime("09");
```

## `DvDateTime`

The most commonly used temporal type. Combines date and time, always with at least
year precision.

```csharp
// Full ISO 8601 datetime with timezone
var recorded = new DvDateTime("2025-05-24T09:15:00+01:00");

// UTC
var utc = new DvDateTime("2025-05-24T08:15:00Z");

// Date only (time not recorded)
var dateOnly = new DvDateTime("2025-05-24");
```

> **Tip** — Always store times with a timezone offset in clinical records.
> `DateTimeOffset.Now.ToString("o")` produces a suitable string.

## `DvDuration`

An ISO 8601 duration (period), used for age, duration of symptoms, dosing intervals, etc.

```csharp
// 1 year, 2 months, 3 days, 4 hours
var age = new DvDuration("P1Y2M3DT4H");

// 30 minutes
var halfHour = new DvDuration("PT30M");

// Negative duration (e.g. pre-conception)
var preConception = new DvDuration("-P280D");
```

| Component | Symbol | Example |
|---|---|---|
| Years | Y | P2Y |
| Months | M | P3M |
| Weeks | W | P1W |
| Days | D | P10D |
| Hours | H | PT12H |
| Minutes | M | PT30M |
| Seconds | S | PT45S |

## Time Specification types

For recurring schedules (medication dosing, appointment series), see
`OpenEHR.RM.DataTypes.TimeSpecification`:

- `DvPeriodicTime` — a repeating schedule described by an iCalendar expression
- `DvGeneralTimeSpecification` — a free-form schedule

---
title: Round-Trip JSON
description: Serialise a Composition to Canonical JSON and deserialise it back.
---

# Round-Trip JSON

This guide demonstrates a full serialise → deserialise round-trip using
`Clarotech.OpenEHR.RM.Json`. It continues from the
[Building a Composition](building-a-composition.md) guide.

## Setup

```bash
dotnet add package Clarotech.OpenEHR.RM.Json
```

```csharp
using Clarotech.OpenEHR.RM.Json;
```

## 1 — Serialise to Canonical JSON

```csharp
// Assume `composition` was built as shown in the Building a Composition guide.

// Compact JSON (for storage/transport)
string json = OpenEhrJsonSerializer.Serialize(composition);

// Pretty-printed (for debugging / display)
string pretty = OpenEhrJsonSerializer.Serialize(composition, writeIndented: true);

Console.WriteLine(pretty);
```

Abbreviated output:

```json
{
  "_type": "COMPOSITION",
  "name": { "_type": "DV_TEXT", "value": "Vital Signs" },
  "archetype_node_id": "openEHR-EHR-COMPOSITION.encounter.v1",
  "language": {
    "_type": "CODE_PHRASE",
    "terminology_id": { "_type": "TERMINOLOGY_ID", "value": "ISO_639-1" },
    "code_string": "en"
  },
  "territory": {
    "_type": "CODE_PHRASE",
    "terminology_id": { "_type": "TERMINOLOGY_ID", "value": "ISO_3166-1" },
    "code_string": "GB"
  },
  "category": {
    "_type": "DV_CODED_TEXT",
    "value": "event",
    "defining_code": {
      "_type": "CODE_PHRASE",
      "terminology_id": { "_type": "TERMINOLOGY_ID", "value": "openehr" },
      "code_string": "433"
    }
  },
  "composer": { "_type": "PARTY_IDENTIFIED", "name": "Dr Alice Smith" },
  "content": [
    {
      "_type": "OBSERVATION",
      "name": { "_type": "DV_TEXT", "value": "Blood Pressure" },
      "archetype_node_id": "openEHR-EHR-OBSERVATION.blood_pressure.v2",
      "data": {
        "_type": "HISTORY",
        "events": [
          {
            "_type": "POINT_EVENT",
            "time": { "_type": "DV_DATE_TIME", "value": "2025-05-25T09:15:00+01:00" },
            "data": {
              "_type": "ITEM_TREE",
              "items": [
                {
                  "_type": "ELEMENT",
                  "name": { "_type": "DV_TEXT", "value": "Systolic" },
                  "value": { "_type": "DV_QUANTITY", "magnitude": 120.0, "units": "mm[Hg]" }
                },
                {
                  "_type": "ELEMENT",
                  "name": { "_type": "DV_TEXT", "value": "Diastolic" },
                  "value": { "_type": "DV_QUANTITY", "magnitude": 80.0, "units": "mm[Hg]" }
                }
              ]
            }
          }
        ]
      }
    }
  ]
}
```

## 2 — Deserialise back

```csharp
var restored = OpenEhrJsonSerializer.Deserialize<Composition>(json);

// Verify round-trip fidelity
Debug.Assert(restored!.Name.Value == composition.Name.Value);

// Access typed values — _type drives concrete type resolution
var obs = (Observation)restored.Content[0];
var tree = (ItemTree)obs.Data.Events[0].Data;
var systolic = (DvQuantity)((Element)tree.Items[0]).Value!;
Console.WriteLine($"Systolic: {systolic.Magnitude} {systolic.Units}"); // 120 mm[Hg]
```

## 3 — Save to and read from a file

```csharp
// Write
await File.WriteAllTextAsync("vital-signs.json", json, Encoding.UTF8);

// Read back
string stored = await File.ReadAllTextAsync("vital-signs.json");
var fromFile  = OpenEhrJsonSerializer.Deserialize<Composition>(stored);
```

## 4 — Async stream round-trip (HTTP POST → GET)

```csharp
using var client = new HttpClient { BaseAddress = new Uri("https://cdr.hospital.example/ehrbase/") };

// POST a new composition
var postContent = new StringContent(json, Encoding.UTF8, "application/json");
var postResponse = await client.PostAsync(
    $"rest/openehr/v1/ehr/{ehrId}/composition", postContent);
postResponse.EnsureSuccessStatusCode();

// GET it back and deserialise from the response stream
var getResponse = await client.GetAsync(
    $"rest/openehr/v1/ehr/{ehrId}/composition/{compositionVersionId}");
getResponse.EnsureSuccessStatusCode();

await using var body = await getResponse.Content.ReadAsStreamAsync();
var cdrComposition = await OpenEhrJsonSerializer.DeserializeAsync<Composition>(body);
```

## 5 — Wrapping in a `Version<Composition>`

When submitting to a CDR that expects versioned payloads:

```csharp
string versionJson = OpenEhrJsonSerializer.Serialize(version, writeIndented: true);

// Deserialise a versioned response
var restoredVersion = OpenEhrJsonSerializer.Deserialize<OriginalVersion<Composition>>(versionJson);
var innerComposition = restoredVersion!.Data;
```

## See also

- [Serialisation](../json/guide/serialization.md)
- [Deserialisation](../json/guide/deserialization.md)
- [Building a Composition](building-a-composition.md)

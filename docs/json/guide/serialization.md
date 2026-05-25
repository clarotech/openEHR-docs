---
title: Serialisation
description: How to serialise openEHR RM objects to Canonical JSON.
---

# Serialisation

```bash
dotnet add package Clarotech.OpenEHR.RM.Json
```

```csharp
using Clarotech.OpenEHR.RM.Json;
```

## Basic usage — `Serialize<T>`

```csharp
// Compact (default)
string json = OpenEhrJsonSerializer.Serialize(composition);

// Pretty-printed
string pretty = OpenEhrJsonSerializer.Serialize(composition, writeIndented: true);
```

Use `Serialize<T>` when the declared type of your variable already is a concrete RM class
(e.g. `Composition`, `DvQuantity`). The `_type` discriminator is derived from `T`.

## Serialising abstract-typed variables — `SerializeByRuntimeType`

When your variable is declared as an abstract or interface type (e.g. `DataValue`,
`ContentItem`, `ItemStructure`) you **must** use `SerializeByRuntimeType` to ensure the
correct `_type` is written:

```csharp
DataValue value = element.Value;   // runtime type: DvQuantity

// ❌ Wrong — emits "_type":"DATA_VALUE" (base class name)
string wrong = OpenEhrJsonSerializer.Serialize(value);

// ✅ Correct — emits "_type":"DV_QUANTITY"
string correct = OpenEhrJsonSerializer.SerializeByRuntimeType(value);
```

## Example output

Given the blood-pressure `Composition` from [Building a Composition](../../integration/building-a-composition.md):

```json
{
  "_type": "COMPOSITION",
  "name": {
    "_type": "DV_TEXT",
    "value": "Vital Signs"
  },
  "archetype_node_id": "openEHR-EHR-COMPOSITION.encounter.v1",
  "archetype_details": {
    "_type": "ARCHETYPED",
    "archetype_id": {
      "_type": "ARCHETYPE_ID",
      "value": "openEHR-EHR-COMPOSITION.encounter.v1"
    },
    "rm_version": "1.1.0"
  },
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
  "composer": {
    "_type": "PARTY_IDENTIFIED",
    "name": "Dr Alice Smith"
  },
  "context": {
    "_type": "EVENT_CONTEXT",
    "start_time": { "_type": "DV_DATE_TIME", "value": "2025-05-25T09:15:00+01:00" },
    "setting": {
      "_type": "DV_CODED_TEXT",
      "value": "primary medical care",
      "defining_code": {
        "_type": "CODE_PHRASE",
        "terminology_id": { "_type": "TERMINOLOGY_ID", "value": "openehr" },
        "code_string": "228"
      }
    }
  },
  "content": [ … ]
}
```

> **Note** — Null-valued properties are omitted. Optional properties that were not set
> (e.g. `uid`, `links`, `feeder_audit`) do not appear in the output.

## Using `OpenEhrJsonOptions` directly

You can pass `OpenEhrJsonOptions.Default` directly to `System.Text.Json` APIs:

```csharp
// Write to a stream (e.g. HttpResponseBody)
await JsonSerializer.SerializeAsync(responseStream, composition, OpenEhrJsonOptions.Default);

// Wrap in custom options
var opts = new JsonSerializerOptions(OpenEhrJsonOptions.Default) { WriteIndented = true };
string json = JsonSerializer.Serialize(composition, opts);
```

## Writing to a file

```csharp
string json = OpenEhrJsonSerializer.Serialize(composition, writeIndented: true);
await File.WriteAllTextAsync("vital-signs.json", json, Encoding.UTF8);
```

## Posting to an openEHR CDR

```csharp
using var client = new HttpClient { BaseAddress = new Uri("https://cdr.hospital.example/ehrbase/") };
client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));

string json = OpenEhrJsonSerializer.Serialize(composition);
var content = new StringContent(json, Encoding.UTF8, "application/json");

var response = await client.PostAsync($"rest/openehr/v1/ehr/{ehrId}/composition", content);
response.EnsureSuccessStatusCode();
```

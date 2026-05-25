---
title: Deserialisation
description: How to deserialise Canonical JSON into openEHR RM objects.
---

# Deserialisation

## From a string

```csharp
using Clarotech.OpenEHR.RM.Json;

string json = /* … */;

var composition = OpenEhrJsonSerializer.Deserialize<Composition>(json);
```

`Deserialize<T>` returns `T?` — it returns `null` when the JSON token is `null`.

## From a stream

```csharp
// Synchronous (e.g. MemoryStream)
using var stream = File.OpenRead("vital-signs.json");
var composition = OpenEhrJsonSerializer.Deserialize<Composition>(stream);

// Asynchronous (e.g. HttpResponseMessage.Content)
using var response = await client.GetAsync(…);
await using var body = await response.Content.ReadAsStreamAsync();
var composition = await OpenEhrJsonSerializer.DeserializeAsync<Composition>(body);
```

## Polymorphic type resolution

The `_type` discriminator field drives concrete-type resolution throughout the object
graph — you do not need to know the concrete type at the call site:

```csharp
// A Composition whose content[] items may be Observation, Evaluation, etc.
var composition = OpenEhrJsonSerializer.Deserialize<Composition>(json);

foreach (var item in composition.Content)
{
    switch (item)
    {
        case Observation obs:
            Console.WriteLine($"Observation: {obs.Name.Value}");
            break;
        case Evaluation eval:
            Console.WriteLine($"Evaluation: {eval.Name.Value}");
            break;
    }
}
```

Likewise for `Element.Value`, which resolves to the correct `DvXxx` class:

```csharp
var element = /* … */;
if (element.Value is DvQuantity qty)
    Console.WriteLine($"{qty.Magnitude} {qty.Units}");
```

## Deserialising a `Version<Composition>`

```csharp
var version = OpenEhrJsonSerializer.Deserialize<OriginalVersion<Composition>>(json);
var composition = version?.Data;
```

## Deserialising an `Ehr`

```csharp
var ehr = OpenEhrJsonSerializer.Deserialize<Ehr>(json);
```

## Case sensitivity

The deserialiser is **case-insensitive** for property names
(`PropertyNameCaseInsensitive = true`), so JSON from CDRs that emit `camelCase`
(e.g. `archetypeNodeId`) is accepted without any configuration.

## Using `OpenEhrJsonOptions` directly

```csharp
// Reading from a Utf8JsonReader
var reader = new Utf8JsonReader(utf8Bytes);
var composition = JsonSerializer.Deserialize<Composition>(ref reader, OpenEhrJsonOptions.Default);

// Reading with custom options layered on top
var opts = new JsonSerializerOptions(OpenEhrJsonOptions.Default);
opts.Converters.Add(new MyExtensionConverter());
var composition = JsonSerializer.Deserialize<Composition>(json, opts);
```

## Error handling

`System.Text.Json` throws `JsonException` on malformed JSON and `NotSupportedException`
when no converter is registered for the target type. A missing `_type` discriminator will
cause `JsonException` when deserialising into an abstract base type such as `DataValue`
or `ContentItem`.

```csharp
try
{
    var composition = OpenEhrJsonSerializer.Deserialize<Composition>(json);
}
catch (JsonException ex)
{
    Console.Error.WriteLine($"Invalid openEHR JSON: {ex.Message}");
}
```

## See also

- [Serialisation](serialization.md)
- [Round-trip JSON](../../integration/round-trip-json.md)

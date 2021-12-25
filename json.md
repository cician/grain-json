---
title: Json
---

JSON (JavaScript Object Notation) parsing and printing.

### Json.**JSON**

```grain
enum JSON {
  JSONNull,
  JSONBoolean(Bool),
  JSONNumber(Number),
  JSONString(String),
  JSONArray(List<JSON>),
  JSONObject(List<(String, JSON)>),
}
```

Data structure representing the result of parsing and the input of printing
JSON.

For example this object
```grain
JSONObject([
("currency", JSONString("€")),
("price", JSONNumber(99.99)),
])
```
is equivalent to the following JSON:
```json
{"currency":"€","price":99.99}
```

This data structure is semantically equivalent to the JSON format allowing
mostly lossless round trips of printing and parsing. Exceptions to this are
white spaces, multiple ways JSON allows escaping characters in strings and
some edge-cases related to Grain's `Number` type.

For example parsing the following JSON text will also result in the same
`JSON` object as above.
```json
{
"currency": "\u20ac",
"price": 99.99
}
```

### Json.**JSONToStringError**

```grain
enum JSONToStringError {
  InvalidNumber(String),
}
```

Represents errors for cases when a `JSON` object cannot be represente in the
JSON format. This can happen when it contains number values `NaN`,
`Infinity` or `-Infinity`.

### Json.**IndentationFormat**

```grain
enum IndentationFormat {
  NoIndentation,
  IndentWithTab,
  IndentWithSpaces(Number),
}
```

Controls how indentation is performed in custom formatting.

Following examples have whitespaces and line breaks replaced with visible
charactes for illustrative purposes.

`NoIndentation`
```json
{↵
"currency":·"€",↵
"price":·99.9↵
}
```

`IndentWithTab`
```json
{↵
→"currency":·"€",↵
→"price":·99.9↵
}
```

`IndentWithSpaces(2)`
```json
{↵
··"currency":·"€",↵
··"price":·99.9↵
}
```

`IndentWithSpaces(4)`
```json
{↵
····"currency":·"€",↵
····"price":·99.9↵
}
```

### Json.**ArrayFormat**

```grain
enum ArrayFormat {
  CompactArrayEntries,
  OneArrayEntryPerLine,
}
```

Controls how arrays are printed in custom formatting.

Following examples have whitespaces and line breaks replaced with visible
charactes for illustrative purposes.

`CompactArrayEntries`
```json
[]
```

```json
[1]
```

```json
[1,2,3]
```

`OneArrayEntryPerLine`
```json
[]
```

```json
[↵
··1↵
]
```

```json
[↵
··1,↵
··2,↵
··3↵
]
```

### Json.**ObjectFormat**

```grain
enum ObjectFormat {
  CompactObjectEntries,
  OneObjectEntryPerLine,
}
```

Controls how objects are printed in custom formatting.

Following examples have whitespaces and line breaks replaced with visible
charactes for illustrative purposes.

`CompactObjectEntries`
```json
{}
```

```json
{"a":1}
```

```json
{"a":1,"b":2,"c":3}
```

`OneObjectEntryPerLine`
```json
{}
```

```json
{↵
··"a": 1↵
}
```

```json
{↵
··"a": 1,↵
··"b": 2,↵
··"c": 3↵
}
```

### Json.**LineEnding**

```grain
enum LineEnding {
  NoLineEnding,
  LineFeed,
  CarriageReturnLineFeed,
  CarriageReturn,
}
```

### Json.**FormattingSettings**

```grain
record FormattingSettings {
  indentation: IndentationFormat,
  arrayFormat: ArrayFormat,
  objectFormat: ObjectFormat,
  lineEnding: LineEnding,
  finishWithNewLine: Bool,
  escapeAllControlPoints: Bool,
  escapeHTMLUnsafeSequences: Bool,
  escapeNonASCII: Bool,
}
```

### Json.**defaultCompactFormat**

```grain
defaultCompactFormat : () -> FormattingSettings
```

### Json.**defaultPrettyFormat**

```grain
defaultPrettyFormat : () -> FormattingSettings
```

### Json.**defaultCompactAndSafeFormat**

```grain
defaultCompactAndSafeFormat : () -> FormattingSettings
```

### Json.**defaultPrettyAndSafeFormat**

```grain
defaultPrettyAndSafeFormat : () -> FormattingSettings
```

### Json.**JSONWriterCompactImplHelper**

```grain
record JSONWriterCompactImplHelper {
  buffer: Buffer.Buffer,
  emitEscapedQuotedString: String -> Void,
}
```

### Json.**JSONWriterPrettyImplHelper**

```grain
record JSONWriterPrettyImplHelper {
  format: FormattingSettings,
  bufferPretty: Buffer.Buffer,
  emitEscapedQuotedStringPretty: String -> Void,
  printNewLine: () -> Void,
  printIndentation: Number -> Void,
}
```

### Json.**JSONWriter**

```grain
record JSONWriter {
  emit: JSON -> Option<JSONToStringError>,
}
```

### Json.**toString**

```grain
toString : (JSON, FormattingSettings) -> Result<String, JSONToStringError>
```

Prints the JSON object into a string with specific formatting settings.







Example output:
```json
{"currency":"\u20ac","price":99.9}
```

Parameters:

|param|type|description|
|-----|----|-----------|
|`json`|`JSON`|The JSON object to print|
|`format`|`FormattingSettings`|Custom formatting setttings|

Returns:

|type|description|
|----|-----------|
|`Result<String, JSONToStringError>`|A `Result` object with either the string containing the printed JSON or an error if the input object cannot be represented in the JSON format.|

Examples:

```grain
print(Result.unwrap(toString(JSONObject([("currency", JSONString("€")), ("price", JSONNumber(99.9))]), defaultCompactAndSafeFormat())))
```

### Json.**toStringCompact**

```grain
toStringCompact : JSON -> Result<String, JSONToStringError>
```

Prints the JSON object into a string with compact formatting optimized for
small size. Recommended for all uses where the intended consumer is a
machine program. For example in REST APIs.

Using this function can be preferred over `toString` with compact formatting
settings since it can be slightly faster.






Example output:
```json
{"currency":"€","price":99.9}
```

Parameters:

|param|type|description|
|-----|----|-----------|
|`json`|`JSON`|The JSON object to print|

Returns:

|type|description|
|----|-----------|
|`Result<String, JSONToStringError>`|A `Result` object with either the string containing the printed JSON or an error if the input object cannot be represented in the JSON format.|

Examples:

```grain
print(Result.unwrap(toStringCompact(JSONObject([("currency", JSONString("€")), ("price", JSONNumber(99.9))]))))
```

### Json.**toStringPretty**

```grain
toStringPretty : JSON -> Result<String, JSONToStringError>
```

Prints the JSON object into a string with default pretty formatting
settings. Recommended for uses where the intended consumer is a person or
the text should be easily inspectable. For example configuration files or
document file formats.




Example output:
```json
{
"currency": "€",
"price": 99.9
}
```

Parameters:

|param|type|description|
|-----|----|-----------|
|`json`|`JSON`|The JSON object to print|

Returns:

|type|description|
|----|-----------|
|`Result<String, JSONToStringError>`|A `Result` object with either the string containing the printed JSON or an error if the input object cannot be represented in the JSON format.|

Examples:

```grain
print(Result.unwrap(toStringPretty(JSONObject([("currency", JSONString("€")), ("price", JSONNumber(99.9))]))))
```

### Json.**JSONParseError**

```grain
enum JSONParseError {
  UnexpectedEndOfInput(String),
  UnexpectedToken(String),
  InvalidUTF16SurrogatePair(String),
}
```

### Json.**JSONParserImplHelper**

```grain
record JSONParserImplHelper {
  reader: StringReader.StringReader,
  currentCodePoint: Number,
  pos: Number,
}
```

### Json.**parse**

```grain
parse : String -> Result<JSON, JSONParseError>
```


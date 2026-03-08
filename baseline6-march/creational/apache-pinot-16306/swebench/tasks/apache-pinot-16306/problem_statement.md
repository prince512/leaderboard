## Description

When working with JSON data stored in Pinot tables, it's often useful to dynamically discover what keys exist in a JSON document without knowing the schema ahead of time. Currently, extracting keys from a JSON field supports only a basic form — returning all matching keys with no control over depth or output format.

Two capabilities are missing:

1. **Depth limiting**: There is no way to restrict key extraction to a maximum nesting depth. For deeply nested JSON documents, this means users are forced to receive every single key at every level, which can be overwhelming and expensive for large documents.

2. **Dot notation output**: Keys are always returned in bracket-based path notation (e.g. a path like $['users'][0]['name']). Many downstream consumers prefer a simpler dot-separated format (e.g. users.0.name) that is easier to read and work with.

## Expected Behavior

- The key extraction function should accept an optional configuration string that allows specifying the maximum depth to recurse and the output format.
- When a maximum depth is specified, only keys at or above that depth should be returned.
- When dot notation output is requested, keys should be returned as dot-separated paths rather than in the default JsonPath bracket notation.
- Backward compatibility must be preserved: queries that do not use the new optional parameter should continue to work exactly as before.
- The function should gracefully handle null input, invalid JSON, empty objects, and empty arrays by returning an empty result.
- The transform function registered in the query engine must also support the new optional parameter.

## Why This Matters

Users exploring semi-structured JSON data need control over key extraction to build schema-discovery queries, introspect large nested documents efficiently, and integrate key paths with other tools or systems that expect a particular format.

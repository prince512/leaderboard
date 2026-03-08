Type: Function
Name: jsonExtractKey
Location: pinot-common/src/main/java/org/apache/pinot/common/function/scalar/JsonFunctions.java
Signature: jsonExtractKey(Object jsonObj, String jsonPath) -> List<String>
Description: Extracts key paths from a JSON object or string using a JsonPath expression. Returns keys in JsonPath bracket notation (e.g. $['a'], $['a']['b']). This 2-parameter form defaults to unlimited depth and JsonPath format. Annotated with @ScalarFunction. Returns empty list for null/invalid/empty input.

Type: Function
Name: jsonExtractKey
Location: pinot-common/src/main/java/org/apache/pinot/common/function/scalar/JsonFunctions.java
Signature: jsonExtractKey(Object jsonObj, String jsonPath, String paramString) -> List<String> throws IOException
Description: Extracts key paths from a JSON object or string with configurable depth and output format. The paramString is a semicolon-delimited set of key=value pairs. Supported keys: 'maxDepth' (integer, negative treated as unlimited), 'dotNotation' (boolean). Unknown keys cause an IllegalArgumentException (wrapped as BadQueryRequestException). Annotated with @ScalarFunction.

Type: Function
Name: convertToDotNotation
Location: pinot-common/src/main/java/org/apache/pinot/common/function/scalar/JsonFunctions.java
Signature: convertToDotNotation(String jsonPath) -> String
Description: Static (package-private or public) method that converts a JsonPath bracket-notation path to dot-separated notation. Rules:
- null, "", or "$" → ""
- $['key'] → key
- $[N] (numeric index) → N (as string)
- $['a']['b'] → a.b
- $['users'][0]['name'] → users.0.name
- Malformed/unclosed patterns after removing '$' are passed through as-is
- Leading dot in result is removed
- Special characters (dashes, underscores, spaces, dots) in key names are preserved

Type: Function
Name: getKeyDepth
Location: pinot-common/src/main/java/org/apache/pinot/common/function/scalar/JsonFunctions.java
Signature: getKeyDepth(String key) -> int
Description: Static (package-private or public) method that returns the nesting depth of a key path. Rules:
- null or "" → 0
- JsonPath bracket notation (starts with "$["): count the number of '[' characters
- Dot notation (contains "."): split by "." and return array length (Java String.split behavior: trailing empty segments discarded, leading/middle empty segments counted)
  - "." → 0 (split produces empty array)
  - "a." → 1 (split("\.") of "a." discards trailing empty, yields ["a"])
  - ".b" → 2 (split("\.") of ".b" yields ["", "b"])
  - "a..b" → 3 (split("\.") of "a..b" yields ["a", "", "b"])
- Single identifier (no brackets, no dots) → 1

Type: Class
Name: JsonExtractFunctionParameters
Location: pinot-common/src/main/java/org/apache/pinot/common/function/scalar/JsonFunctions.java
Description: Inner public static class of JsonFunctions that parses the options parameter string. Fields: _maxDepth (int, default Integer.MAX_VALUE), _dotNotation (boolean, default false). Constructor: JsonExtractFunctionParameters(String parametersString). Methods: getMaxDepth() -> int, isDotNotation() -> boolean. Parsing: splits by ';', then by '=', trims whitespace, converts key to uppercase. Valid keys: "MAXDEPTH", "DOTNOTATION". Negative maxDepth is converted to Integer.MAX_VALUE. Unknown keys throw IllegalArgumentException.

Type: Class
Name: JsonExtractKeyTransformFunction
Location: pinot-core/src/main/java/org/apache/pinot/core/operator/transform/function/JsonExtractKeyTransformFunction.java
Description: Existing transform function class that must be updated to accept 2 or 3 arguments (previously only 2). The constant FUNCTION_NAME = "jsonExtractKey" must remain. Returns multi-value STRING DataType. When 3 arguments are provided, the 3rd must be a literal string parsed as options. Unknown option keys cause BadQueryRequestException. 1 argument or 4+ arguments cause SqlCompilationException at parse time.

Type: Enum update
Name: TransformFunctionType.JSON_EXTRACT_KEY
Location: pinot-common/src/main/java/org/apache/pinot/common/function/TransformFunctionType.java
Description: The JSON_EXTRACT_KEY entry must be updated to accept 2 or 3 arguments (CHARACTER type), making the 3rd argument optional (i -> i > 1 predicate for optional args).

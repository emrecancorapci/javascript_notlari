# Coercion (Tür Dönüşümü) Tablosu

## String: `ToString(value)` : `ToPrimitive(value, String)`

| Input Type                | Output            |
|---------------------------|-------------------|
| 0                         | "0"               |
| -0                        | "0"               |
| []                        | ""                |
| [1,2,3]                   | "1,2,3"           |
| [null, undefined]         | ","               |
| [[[],[],[]],[]]           | ",,,"             |
| [,,,,]                    | ",,,"             |
| {}                        | "[object Object]" |
| {a: 1}                    | "[object Object]" |
| {toString: () => "foo"}   | "foo"             |

## Number: `ToNumber(value)` : `ToPrimitive(value, Number)`

### numbers

| Input Type                | Output            |
|---------------------------|-------------------|
| ""                        | 0                 |
| " "                       | 0                 |
| "0"                       | 0                 |
| "0."                      | 0                 |
| ".0"                      | 0                 |
| "-0"                      | -0                |
| "  007  "                 | 7                 |
| "0xad"                    | 173               |
| "0b11"                    | 3                 |
| "."                       | NaN               |

### other primitive types

| Input Type                | Output            |
|---------------------------|-------------------|
| "foo"                     | NaN               |
| **null***                 | 0                 |
| **undefined***            | NaN               |
| {..}                      | NaN               |

### arrays

| Input Type                | Output            |
|---------------------------|-------------------|
| []                        | 0                 |
| [""]                      | 0                 |
| ["42"]                    | 42                |
| ["0"]                     | 0                 |
| ["-0"]                    | -0                |
| [null]                    | 0                 |
| [undefined]               | 0                 |
| [1,2,3]                   | NaN               |

## Boolean: `ToBoolean(value)` : `ToPrimitive(value, Boolean)`

| Input Type                | Output            |
|---------------------------|-------------------|
| ""                        | false             |
| 0                         | false             |
| -0                        | false             |
| NaN                       | false             |
| null                      | false             |
| undefined                 | false             |
| ***Everything else***     | true              |

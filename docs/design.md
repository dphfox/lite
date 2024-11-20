
# Type system

Lite is designed around a carefully chosen set of types, with particular predictable characteristics.

## Symbols

| Unsized | Owned |
|---------|-------|
| no      | yes   |

Symbols are unique types that only equal themselves. They only have one variant, so are always equal to themselves.

They are defined using the `symbol` keyword, which must appear alone in a type definition.

```luau
type nil = symbol
type true = symbol
type false = symbol
```

Lite has built-in `nil`, `true` and `false` symbols.

## Values

| Unsized | Owned |
|---------|-------|
| no      | yes   |

Values carry opaque, owned data. They don't just have one variant, so aren't always equal to themselves.

They are defined using the `value` keyword, which must appear alone in a type definition.

```luau
type number = value
type string = value
type vector = value
```

Lite has built-in `number`, `string` and `vector` values.

## Structs

| Unsized     | Owned |
|-------------|-------|
| if contents | yes   |

Structs describe a named group of values. They are uniquely identified by shape.

They are defined like a Luau table with only explicitly named fields.

```luau
type person = {
    name: string,
    age: number,
    size: vector
}
```

## Unions

| Unsized     | Owned |
|-------------|-------|
| if contents | yes   |

Unions describe a value that could be one of many types.

They are defined using the bar `|` operator.

```luau
type bool = true | false
```

Lite has a built-in `bool` union.

The question mark `?` operator is syntax sugar for a union with the built-in `nil` symbol.

```luau
type trit = bool?
```

## Tuples

| Unsized     | Owned |
|-------------|-------|
| if contents | yes   |

Tuples describe a positional list of values. They are uniquely identified by shape.

They are defined in parentheses `()`. Trailing commas are allowed for disambiguation.

```luau
type complex = (number, number)
type quaternion = (number, vector)
type just_a_number = (number,)
```

## Arrays

| Unsized                       | Owned   |
|-------------------------------|---------|
| if contents, or no max length | yes     |

Arrays describe a list of values. They are uniquely identified by shape.

They are defined like a Luau array:

```luau
type people = {person}
```

A maximum length may be defined by starting the declaration with a length.

```luau
type couple = {2 person}
```
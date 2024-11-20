
# Type system

Lite is designed around a carefully chosen set of types, with particular predictable characteristics.

## Symbols

| Unsized |
|---------|
| no      |

Symbols are unique types that only equal themselves. They only have one variant, so are always equal to themselves.

They are defined using the `symbol` keyword, which must appear alone in a type definition.

```luau
type nil = symbol
type true = symbol
type false = symbol
```

Lite has built-in `nil`, `true` and `false` symbols.

## Never

| Unsized |
|---------|
| no      |

The `never` type represents an unsatisfiable type - it is never equal to anything.

```luau
type never = -- nothing at all
```

## Values

| Unsized |
|---------|
| no      |

Values carry opaque, owned data. They don't just have one variant, so aren't always equal to themselves.

They are defined using the `value` keyword, which must appear alone in a type definition.

```luau
type number = value
type string = value
type vector = value
```

Lite has built-in `number`, `string` and `vector` values.

## Structs

| Unsized     |
|-------------|
| if contents |

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

| Unsized     |
|-------------|
| if contents |

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

## Arrays

| Unsized                       |
|-------------------------------|
| if contents, or no max length |

Arrays describe a list of values. They are uniquely identified by shape.

They are defined like a Luau array:

```luau
type people = {person}
```

A maximum length may be defined by starting the declaration with a length.

```luau
type couple = {2 person}
```

## References

| Unsized |
|---------|
| no      |

References point to dynamic data that can come from anywhere in the program. 

The reference is conceptually a "wide pointer", allowing unsized data to be stored in sized objects.

References are defined with the `ref` keyword:

```luau
type far_away_person = ref person
```

## Unknown

| Unsized |
|---------|
| yes     |

The `unknown` type represents the union of all possible expressible types. This includes unsized types, so it is also unsized.

```luau
type unknown = -- everything possible, unioned together
```
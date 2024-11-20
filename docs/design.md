
# Type system

Lite's type system is designed around a foundational core that can always be statically resolved and sized.

Everything is defined as a composition of minimal foundations, requiring some extra types which aren't found or exposed publicly in Luau.

Beyond this, a set of types are added on top to deal with dynamic dispatch, arbitrary-size data, and shared data.

## Symbols

| Sized | Shared |
|-------|--------|
| yes   | no     |

Symbols are unique types that only equal themselves. They only have one variant, so are always equal to themselves.

They are defined using the `symbol` keyword, which must appear alone in a type definition.

```luau
type nil = symbol
type true = symbol
type false = symbol
```

Lite has built-in `nil`, `true` and `false` symbols.

## Values

| Sized | Shared |
|-------|--------|
| yes   | no     |

Values carry opaque, owned data. They don't just have one variant, so aren't always equal to themselves.

They are defined using the `value` keyword, which must appear alone in a type definition.

```luau
type number = value
type string = value
type vector = value
```

Lite has built-in `number`, `string` and `vector` values.

## References

| Sized | Shared |
|-------|--------|
| yes   | no     |

References describe a pointer to some other data. References have a fixed size, but can point to any size data.

They are defined using the `dyn` operator.

```luau
type far_away_person = dyn person
```

## Structs

| Sized               | Shared |
|---------------------|--------|
| if all contents are | no     |

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

| Sized               | Shared |
|---------------------|--------|
| if all contents are | no     |

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

| Sized               | Shared |
|---------------------|--------|
| if all contents are | no     |

Tuples describe a positional list of values. They are uniquely identified by shape.

They are defined in parentheses `()`. Trailing commas are allowed for disambiguation.

```luau
type complex = (number, number)
type quaternion = (number, vector)
type just_a_number = (number,)
```

## Arrays

| Size                              | Shared |
|-----------------------------------|--------|
| if contents are + has max length  | no     |

Arrays describe a list of values. They are uniquely identified by shape.

They are defined like a Luau array:

```luau
type people = {person}
```

A maximum length may be defined by starting the declaration with a length.

```luau
type couple = {2 person}
```

# Design of the Lite language

## Type system

Lite is designed around "sized" and "unsized" types:

- **Sized** types have a definite number of fields at compile time, allowing them to be unpacked.
- **Unsized** types don't have a definite number of fields, requiring them to be heap allocated.

The core types are carefully chosen to ensure that sized and unsized data is handled consistently.

### Symbols

| Unsized |
|---------|
| no      |

Symbols are unique types that only equal themselves. They only have one variant, so are always equal to themselves.

Lite has built-in `nil`, `true` and `false` symbols.

### Never

| Unsized |
|---------|
| no      |

The `never` type represents an unsatisfiable type - it is never equal to anything.

### Values

| Unsized |
|---------|
| no      |

Values carry opaque, owned data. They don't just have one variant, so aren't always equal to themselves.

Lite has built-in `number`, `string` and `vector` values.

### Structs

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

### Unions

| Unsized     |
|-------------|
| if contents |

Unions describe a value that could be one of many types.

They are defined using the bar `|` operator.

```luau
type boolean = true | false
```

Lite has a built-in `boolean` union.

The question mark `?` operator is syntax sugar for a union with the built-in `nil` symbol.

```luau
type trit = boolean?
```

### Arrays

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

### Maps

| Unsized |
|---------|
| yes     |

Maps are a set of key/value pairs. Because the size of the set is unknown, maps have no definite size.

They are defined like a Luau table with a single indexer:

```luau
type people_by_name = {[string]: person}
```

### Unknown

| Unsized |
|---------|
| yes     |

The `unknown` type represents the union of all possible expressible types. This includes unsized types, so it is also unsized.

```luau
type unknown = -- everything possible, unioned together
```

### References

| Unsized |
|---------|
| no      |

References point to dynamic data that can come from anywhere in the program. 

The reference is conceptually a "wide pointer", allowing unsized data to be stored in sized objects.

References are typically inferred when dealing with unsized data, but this can be turned off.

### Function references

| Unsized |
|---------|
| no      |

Function references point to a closure somewhere in the program.

Function references are defined using the arrow `() -> ()` operator. For single returns, the parentheses can be omitted.

```luau
type do_something = () -> ()
type do_something_else = (x: number) -> string
type do_something_crazy = (x: string, y: boolean) -> (person, vector)
```

### Generics

| Unsized                      |
|------------------------------|
| if contents & parameters are |

Generics allow a family of types to be expressed.

Generics are defined with angle brackets and a list of identifiers.

```luau
type binding<interior> = {value: interior}?
```

## Language spec

These are strictly the features supported by Lite.

### Literals

Number, string, boolean and nil literals are supported.

There are three kinds of table literal. Arrays, which use a list syntax:

```luau
local foo: {number | string} = {1, "foo", 2}
```

Structs, which use unbracketed names:

```luau
local billy: person = {
    name = "Billy",
    age = 15,
    size = vector.create(0.8, 1.6, 0.5)
}
```

And maps, which use bracketed expressions:

```luau
local people: people_by_name = {
    [billy.name] = billy
}
```

They may not be mixed.

Function literals are supported:

```luau
local fn: do_something_else = function(x: number): string
    return "The number is " .. tostring(x)
end
```

### Definitions

Local variables can be defined with identifier and type:

```
local foo: number
```

They may later be assigned to anything of that type only:

```
foo = 5
```

The two can be combined:

```
local foo: number = 5
```

Local functions may also be defined with typed parameters and returns:

```
local function foo(x: number): string
    return "The number is " .. tostring(x)
end
```

### Control flow

All Luau control flow is supported.

### Expressions

All Luau expressions are supported.

### Type annotations

Type annotations can be omitted. The compiler will try to infer the type, but failing that, will fall back to `unknown`.

All locations in a Luau program expect sized values only. Unsized values are automatically referenced.

Forcing a type with `::` forces the type to become referenced. 

It is not possible to "dereference" a type, but it is possible to perform an operation on a referenced type that results in a new, non-referenced value. For example, getting the length of a referenced table returns a number which is not referenced.
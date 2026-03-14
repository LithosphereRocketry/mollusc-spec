# MOLLUSC Instruction Reference

## Conventions

Instruction arguments in this reference are listed in the format `name:type`.
Instruction argument types are one of the following:

* `reg`: General-purpose register
* `i10`: 10-bit signed immediate
* `j22`: 22-bit signed relative jump (word-aligned, resulting in a value range
    of 2^24 words)
* `u22`: 22-bit upper immediate (10-bit aligned, resulting in a full 32-bit
    value range)
* `pr`: Predicate register

Multiple instructions may appear per entry if they share a mnemonic, even if
their encodings are different. For example, `add q:reg, a:reg, b:reg` and
`add q:reg, a:reg, b:i10` are listed together.

## Instructions by Type

### Integer Arithmetic

#### `add`

`add q:reg, a:reg, b:reg`, `add q:reg, a:reg, b:i10`

Adds the values of `a` and `b` and places the result in `q`.

## Alphabetical index

* [`add`](#add)

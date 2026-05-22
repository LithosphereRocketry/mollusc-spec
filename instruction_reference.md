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

Encodings are specified in bits, from MSB to LSB.

All operations involving predicate values can be inverted, either at the input
or output stage (*for now) by prefixing the predicate value with ! instead of ?.
This is encoded using the `In` bit in the instruction, where a value of 1
denotes uninverted and a value of 0 denotes inverted. (This polarity is chosen
so that the zero value is "always true" rather than "always false".)

All instructions may be prefixed by a conditional of the form `?c:pr` (run if
predicate true) or `!c:pr` (run if predicate false). These are omitted from the
instruction description for clarity.

## Instructions by Type

### Integer Arithmetic

#### `add`

`add q:reg, a:reg, b:reg`, `add q:reg, a:reg, b:i10`

Adds the values of `a` and `b` and places the result in `q`.

##### Encodings

```
Bit
31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1  0
In [  pr  ] [  q:reg  ]  0  0  0  0  0  0  0  0 [  a:reg  ]  0  0  x  x  x  x  x  x [  b:reg  ]
In [  pr  ] [  q:reg  ]  0  0  0  0  0  0  0  0 [  a:reg  ]  0  1 [           b:i10           ]
```

#### `and`

`and q:reg, a:reg, b:reg`, `and q:reg, a:reg, b:i10`

Bitwise-ands the values of `a` and `b` and places the result in `q`.

##### Encodings

```
Bit
31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1  0
In [  pr  ] [  q:reg  ]  0  0  0  0  0  0  1  0 [  a:reg  ]  0  0  x  x  x  x  x  x [  b:reg  ]
In [  pr  ] [  q:reg  ]  0  0  0  0  0  0  1  0 [  a:reg  ]  0  1 [           b:i10           ]
```

### Comparison

#### `eq`

`eq q:pr, a:reg, b:reg`, `eq q:pr, a:reg, b:i10`

Sets `q` if `a` is equal to `b`, and clears it otherwise.

##### Encodings

```
Bit
31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1  0
In [  pr  ] [  q:reg  ]  0  0  1  0  0  0  1  0 [  a:reg  ]  0  0  x  x  x  x  x  x [  b:reg  ]
In [  pr  ] [  q:reg  ]  0  0  1  0  0  0  1  0 [  a:reg  ]  0  1 [           b:i10           ]
```

#### `bit`

`bit q:pr, a:reg, b:reg`, `bit q:pr, a:reg, b:i10`

Sets `q` equal to the `b`th bit of `a`, where bit 0 is the least significant bit.

##### Encodings

```
Bit
31 30 29 28 27 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10  9  8  7  6  5  4  3  2  1  0
In [  pr  ] [  q:reg  ]  0  0  1  0  0  0  1  1 [  a:reg  ]  0  0  x  x  x  x  x  x [  b:reg  ]
In [  pr  ] [  q:reg  ]  0  0  1  0  0  0  1  1 [  a:reg  ]  0  1 [           b:i10           ]
```


### Constant Loading

## Alphabetical index

* [`add`](#add)
* [`and`](#and)
* [`bit`](#bit)
* [`eq`](#eq)

<!--
ecall
hcall
j
jx
jxe
jxi
ld
ltu
lt
lui
lur
or
sl
sr
sra
st
sub
xor
-->
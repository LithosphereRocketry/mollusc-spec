# MOLLUSC Core Architecture

## Introduction

MOLLUSC is a 32-bit RISC architecture designed for low resource requirements for
both emulation and hardware, ability to implement basic virtual-memory operating
systems, and simplicity of design.

### Notes

At times instruction sequences need to refer to inaccessible temporary values
used internally to the processor. These values will be refered to as TMP#, where
\# represents any nonnegative integer. TMP values between different sequences
are unrelated and should not be assumed to be related in function.

## Structure overview

MOLLUSC is a load-store architecture with 16 32-bit general-purpose registers,
the lowermost of which is fixed to always zero. It uses a flat 32-bit address
space for code, data, and peripherals, as well as independent address spaces for
configuration registers and, in some implementations, TLBs.

## Main address space

MOLLUSC uses a 32-bit, byte-addressed address space. 32-bit-aligned word
accesses are always supported; implementations may optionally choose to support
halfword and byte accesses and/or unaligned access (see Feature Detection
below.) Words are stored in "little-endian" format; that is, a word is referred
to by the same address as its least significant byte or halfword.

## Configuration registers

Configuration registers occupy a separate address space from main memory. This
space has two main behavioral differences as compared to main memory:

* Configuration registers are local to the logical processor they are accessed
  from. No processor may observe or modify another processor's configuration
  registers.
* Configuration registers are word-addressed, rather than byte-addressed. It is
  not possible to read or write only part of a configuration register, and
  configuration register 0x4 refers to the fifth (zero-indexed) configuration
  register, rather than the second.

### Feature Detection

Feature detection is performed using the first four configuration registers.
Their fields are defined as follows:

| CR0 Bit | Feature                 |
| ======= | ======================= |
| 0 (LSB) | Feature Detection (CR0) |
| 1       | Feature Detection (CR1) |
| 2       | Feature Detection (CR2) |
| 3       | Feature Detection (CR3) |
| 4       | Sized Access            |
| 5-31    | Reserved                |

Note that each feature detection register is, itself, an optional feature.
All implementations are required to support the `ld.cr` instruction used to
query the feature detection CRs, but if the CR0-CR3 feature bits are not set,
values other than the Feature Detection bit in the respective register should be
considered undefined, and all extensions considered disabled. (A side effect of
this is that an implementation of `ld.cr` that always returns a low bit of 0 is
a compliant implementation; additionally, simple designs that only support
features in CR0 can return a static value ending in `0b0001`.)

## Startup sequence

All MOLLUSC processors start up with the following sequence:

* Load a word from physical address 0x00000000 into TMP0.
* Set PC to TMP0.
* Fetch and execute the instruction at PC, and continue execution as normal.

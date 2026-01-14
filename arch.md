# MOLLUSC Core Architecture

MOLLUSC is a 32-bit RISC architecture designed for low resource requirements for
both emulation and hardware, ability to implement basic virtual-memory operating
systems, and simplicity of design.

## Structure

MOLLUSC is a load-store architecture with 16 32-bit general-purpose registers,
the lowermost of which is fixed to always zero. It uses a flat 32-bit address
space for code, data, and peripherals, as well as independent address spaces for
configuration registers and, in some implementations, TLBs.

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

## Feature Detection

Feature detection is performed using the first four configuration registers.
Their fields are defined as follows:

| CR0 Bit | Feature           |
| ======= | ================= |
| 0 (LSB) | Feature Detection |

Note that feature detection is, itself, an optional feature. All implementations
are required to support the `ld.cr` instruction used to query the feature
detection CRs, but values other than the Feature Detection bit should be
considered undefined, and all extensions considered disabled. (A side effect of
this is that an implementation of `ld.cr` that always returns a low bit of 0 is
a compliant implementation.)
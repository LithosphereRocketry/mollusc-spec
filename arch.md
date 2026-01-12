# MOLLUSC Core Architecture

MOLLUSC is a 32-bit RISC architecture designed for low resource requirements for
both emulation and hardware, ability to implement basic virtual-memory operating
systems, and simplicity of design.

# Structure

MOLLUSC is a load-store architecture with 16 32-bit general-purpose registers,
the lowermost of which is fixed to always zero.
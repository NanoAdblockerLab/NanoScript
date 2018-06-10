# NanoScript

Experimental bytecode virtual machine


## Overview

NanoScript is a scripting language that is designed to run in a bytecode
virtual machine implemented in JavaScript.

The syntax depends on the compiler. See the compiler documentation for more
details. This document is designed for compiler developers.

When compiled, the texual code will be converted to a bytecode, each byte can
have value of 0 to 127 in order to ensure encoding goes smoothly.


## Glossary

### `arg[i]`

The arguments.

### `d[i]`

The data storage, there must be at least 4 slots. Each slot contains a string,
these strings must not be modifiable in runtime.

### `ip`

The instruction pointer, it points to the **next** instruction to fetch. The
script exits when `ip` points beyond the last byte of instructions buffer.

### `r[i]`

The registers, there must be at least 4 registers, each register hold a
JavaScript value that can be modified in runtime.


## Instructions - Basic

### `EXIT` - Exit

Opcode: `0b0000000`

0 arguments expected

Stop execution of current script.

### `LOAD` - Load

Opcode: `0b0000010`

2 arguments expected

`arg[0]` - The source data storage number

`arg[1]` - The target register number

Copy the value in source to the target.

`Invalid Instruction` will be raised if the data storage number or the
register number is invalid, and the faulty script will be terminated.

### `MOVE` - Move

Opcode: `0b0000011`

2 arguments expected

`arg[0]` - The source register number

`arg[1]` - The target register number

Copy the value in source to the target.

`Invalid Instruction` will be raised if either register number is invalid, and
the faulty script will be terminated.

### `SYSC` - System call

Opcode: `0b0000001`

1 argument expected

`arg[0]` - The system call number

Perform a system call, arguments to the system function are to be passed
through registers. See documentation of system calls for more details.

`Invalid Instruction` will be raised if the call number is invalid, and the
faulty script will be terminated.


## Instructions - Control Flow

### `EXIF` - Exit if

Opcode: `0b0100000`

1 argument expected

`arg[0]` - The register number to assert

If the register in question holds a truthy value, stop execution of current
script.

`Invalid Instruction` will be raised if the register number is invalid, and the
faulty script will be terminated.

### `EXIN` - Exit if not

Opcode: `0b0100001`

1 argument expected

`arg[0]` - The register number to assert

If the register in question holds a falsy value, stop execution of current
script.

`Invalid Instruction` will be raised if the register number is invalid, and the
faulty script will be terminated.

### `JPIF` - Jump if

Opcode: `0b0100010`

2 arguments expected

`arg[0]` - The register number to assert

`arg[1]` - The offset, must be a strictly positive integer

If the register in question holds a truthy value, add `arg1` to `ip`.

`Invalid Instruction` will be raised if the register number is invalid, and the
faulty script will be terminated.

### `JPIN` - Jump if not

Opcode: `0b0100011`

2 arguments expected

`arg[0]` - The register number to assert

`arg[1]` - The offset, must be a strictly positive integer

If the register in question holds a falsy value, add `arg1` to `ip`.

`Invalid Instruction` will be raised if the register number is invalid, and the
faulty script will be terminated.


## Instructions - Special

### `IINV` - Invalid instruction

Opcode: Any undefined opcode

0 arguments expected

`Invalid Instruction` will be raised, and the faulty script will be terminated.

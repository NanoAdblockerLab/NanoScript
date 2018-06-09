# NanoScript

Experimental bytecode virtual machine


## Overview

NanoScript is a scripting language that runs on top of a virtual machine
implemented in JavaScript.

The syntax is really simple and straightforward:
```
INSTRUCTION arg0 arg1 ...
```

When compiled, the texual code will be converted to a bytecode, each byte can
have value of 0 to 127 in order to ensure encoding goes smoothly.


## Glossary

### `arg0`, `arg1`, ...

The first, second, ... argument

### `d0`, `d1`, `d2`, `d3`

The first, second, third, fourth data storage. There are a total of four data
storages.

### `ip`

The instruction pointer, it points to the **next** instruction to fetch.

### `r0`, `r1`, ..., `r7`

The first, second, ..., eighth register. There are a total of 8 registers.


## Instructions - Basic

### `EXIT` - Exit

Opcode: `0b0000000`

0 arguments expected

Stop execution of current script.

### `LOAD` - Load

Opcode: `0b0000010`

2 arguments expected

`arg0` - The source data storage number
`arg1` - The target register number

Copy the value in source to the target.

`Invalid Instruction` will be raised if the data storage number or the 
register number is invalid, and the faulty script will be terminated.

### `MOVE` - Move

Opcode: `0b0000011`

2 arguments expected

`arg0` - The source register number
`arg1` - The target register number

Copy the value in source to the target.

`Invalid Instruction` will be raised if either register number is invalid, and
the faulty script will be terminated.

### `SYSC` - System call

Opcode: `0b0000001`

1 argument expected

`arg0` - The system call number

Perform a system call, arguments to the system function are to be passed
through registers. See documentation for system calls for more details.

Depending on the execution context, different system functions are available.
No two functions can have the same number, even if they will never be
available at the same time.

`Invalid Instruction` will be raised if the call number is invalid, and the
faulty script will be terminated.

## Instructions - Control Flow

### `EXIF` - Exit if

Opcode: `0b0100000`

1 argument expected

`arg0` - The register number to assert

If the register in question holds a truthy value, stop execution of current
script.

`Invalid Instruction` will be raised if the register number is invalid, and the
faulty script will be terminated.

### `EXIN` - Exit if not

Opcode: `0b0100001`

1 argument expected

`arg0` - The register number to assert

If the register in question holds a falsy value, stop execution of current
script.

`Invalid Instruction` will be raised if the register number is invalid, and the
faulty script will be terminated.

### `JPIF` - Jump if

Opcode: `0b0100010`

2 arguments expected

`arg0` - The register number to assert
`arg1` - The offset, must be a strictly positive integer

If the register in question holds a truthy value, add `arg1` to `ip`.

`Invalid Instruction` will be raised if the register number is invalid, and the
faulty script will be terminated.

`Segmentation Fault` will be raised if `ip` points beyond the end of
instructions buffer after the addition, and the faulty script will be
terminated.

### `JPIN` - Jump if not

Opcode: `0b0100011`

2 arguments expected

`arg0` - The register number to assert
`arg1` - The offset, must be a strictly positive integer

If the register in question holds a falsy value, add `arg1` to `ip`.

`Invalid Instruction` will be raised if the register number is invalid, and the
faulty script will be terminated.

`Segmentation Fault` will be raised if `ip` points beyond the end of
instructions buffer after the addition, and the faulty script will be
terminated.

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

## Instructions - Basic Control

### `EXIT` - Exit

Opcode: `0`

0 arguments expected

Stop execution of current script.

### `JPIF` - Jump if

Opcode: `10`

2 arguments expected

`arg0` - The register number to assert
`arg1` - The offset, must be a strictly positive integer

If `arg0` is a truthy value, add `arg0` to `ip`.

### `JPIN` - Jump if not

Opcode: `11`

2 arguments expected

`arg0` - The register number to assert
`arg1` - The offset, must be a strictly positive integer

If `arg0` is a falsy value, add `arg0` to `ip`.

### `EXIF` - Exit if

Opcode: `12`

1 argument expected

`arg0` - The register number to assert

If `arg0` is a truthy value, stop execution of current script.

### `EXIF` - Exit if not

Opcode: `13`

1 argument expected

`arg0` - The register number to assert

If `arg0` is a falsy value, stop execution of current script.

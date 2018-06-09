# NanoScript

Experimental bytecode virtual machine

## Overview

NanoScript is a scripting language that runs on top of a virtual machine
implemented in JavaScript.

The syntax is really simple and straightforward:
```
INSTRUCTION arg0 arg1 ...
```

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

### `EXIT`

0 arguments expected

Stop execution of current script.

### `JMPIF`

2 arguments expected

`arg0` - The register number to assert
`arg1` - The offset, must be a strictly positive integer

If `arg0` is a truthy value, add `arg0` to `ip`.

### `

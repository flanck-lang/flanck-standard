# flanck-standard
The flanck standard v1.0.0.

## Idea

### BDS (Binary Data Stack)

- BDS is the only data type in flanck
- it is a list bits (`0`|`1`), that can be any length
- as like any stack the BDS has a top
> **_NOTE:_** The '→' denotes the top of the stack
```
→010101
```
- a BDS is immutable
- for a stack `s` with `n` bits,
  `s(x)` refers to to the bit at the index `x`,
  which starts with `0` at the top and `s(n - 1)`
  for the last bit (the bottom)
- for a stack `s` with length `n = 0`, `s(0)` cannot exist

### Stack holders

- stack holders contain a BDS
- a flanck program has a known number of stack holders at compile time

#### Checking a stack holder

Checking stack holders means,
looking if a BDS is contained at the top of the BDS in the stack holder

→ for a BDS `s1` with length `n1` and a BDS in the stack holder `s2` with length `n2`

1. Condition: `n2` must be greater or equal to `n1`
2. Condition: for `x` in `0...(n1 - 1)`, `s1(x)` has to be equal to `s2(x)`

#### Removing from a stack holder

From a stack holder, a BDS can be removed.

→ for a BDS `s1` with length `n1` and a BDS in the stack holder `s2` with length `n2`

The stack holder is given a new BDS, which contains all elements of s2 except the first n1 elements.

#### Writing to a stack holder

In the stack holder a BDS can be put on top.

→ for a BDS `s1` with length `n1` and a BDS in the stack holder `s2` with length `n2`

The stack holder is given a new BDS,
which contains the bits `s1(0)...s1(n1-1) + s2(0)...s2(n2-1)`

### Instruction

- a flanck program consists of a list of instructions
- each instruction contains BSDs to check stack holders, each stack holder can be checked once or not
- if all columns are successfully checked, they are then removed by the BSD with which they were checked
- each instruction then contains BSDs to be written to stack holders,
  if all stack holders are successfully checked
- the number of stack holders a program has is defined by
  how many stack holders are removed, writen and checked by the instructions

### Proper execution of an instruction

- proper execution can either mean all checks of an instruction succeeded
  or if a instruction mutated any columns
- mutated any column, means that the instruction can have removed bits of one column but added the same bits to it in the same instruction
```js
// counts as proper mutation of columns
[1]:[1]
```
> **_NOTE:_** The syntax will be explained later on in detail.

### Program flow

- a flanck program consists of a list of instructions
- the flanck program is run from the top down and each instruction is executed
- if at least one instruction is executed proper,
  after all instructions have ben run,
  the instructions will be run again from the top down again,
  until no instruction was executed proper

## Syntax

### Generals

flanck programs cannot have any syntax errors, only characters can be ignored.

#### Instruction generals

- an instruction consists of the BDS definitions to check
  and remove from the stack holders (CBDs),
  as well as BDS definitions to write to the stack holders (WBDs)
- the stack holders identity is determined by the order,
  in which it is stated
- the nth CBD, will refer to the same stack holder as the nth WBD
- there can be a different number or CBDs than WBDs,
  there can even be no CBDs and no WBDs
- CBD and WBD are seperated by a colon ':'
- instructions are seperated by new lines
- if a line does not contain ':' it is not a valid instruction and will be ignored
- the syntax of the CBD and WBD seperators are defined
  in the **Standart Syntax** and **Modern Syntax** sections
- the data (BDS) inside of CBD or WBDs is a list of 0 and 1, with 0 representing off and 1 on
- the first element from the left is the top of the BDS
- if a CBD or WBD contains no 0s or 1s it is empty

#### Comments/ignored characters

- all characters that are not part of valid flanck syntax are ignored
  and can be used as comments
- it does not matter if these characters are between other valid characters or not
- it is advised to use some kind of convention for comments

### Standard syntax

The classic syntax, has no limitations but is a verbose.

#### CBD and WBD seperators

- the data of a CBD or WBD is encapsulated between two squared brackets '\[' and '\]'
- the instruction `:` means that there are no CBDs or WBDs
```js
[00][1]:[1] // two CBDs containing 00 and 1 and one WBD containing 1
```

### Modern syntax 

The modern syntax, is easier to read, but has small limitations.

#### CBD and WBD seperators

- different CBDs or WBDs are seperated by the pipe sybmol '|'
```js
00|1:1 // two CBDs containing 00 and 1 and one WBD containing 1
```
- there always is at least one CBD or WBD
```js
00|1: // two CBDs containing 00 and 1 and one empty WBD  
```

### Input and output

This document does not define any standard for input or output,
but any columns can be used for input or ouput.

# Puzzle 3

  Location  Bytecode  Opcode name  
      00      36      CALLDATASIZE
      01      56      JUMP
      02      FD      REVERT
      03      FD      REVERT
      04      5B      JUMPDEST
      05      00      STOP  

## About the puzzle

CALLDATASIZE (36) will get the size (in bytes) of the calldata in the current environment and place it on the stack. We want to reach JUMPDEST (5B) at location 04 by passing in a value to JUMP (56).  CALLDATASIZE will place a value on the stack. The PC will increment by 1 and move to the next instruction, JUMP.

JUMP will take the top value from the stack (in this case will it be the value that was pushed to the stack by CALLDATASIZE) and jump to its location.  The location needs to be a JUMPDEST instruction.

Our goal is to enter a value with a data size (in bytes) that will take us to location 04. We will need to enter a value of 4 bytes to jump to location 04.

We need to pass in a value that is 4 bytes. We will use 0x00000000.  Remember that ONE byte is TWO hex characters. 00 00 00 00 = 4 bytes.

### 1. CALLDATASIZE

The value 0x00000000 is passed in to the transaction.  CALLDATASIZE gets the size of the calldata (in bytes) and places it on the stack.  The value we passed in has a size of 4 bytes.
    Location  Bytecode  Opcode name  
        00      36      CALLDATASIZE <--- Pass in 0x00000000 as calldata
        01      56      JUMP
        02      FD      REVERT
        03      FD      REVERT
        04      5B      JUMPDEST
        05      00      STOP  

    Current Stack:
    Empty

### 2. JUMP

JUMP pops the top value of the stack and jumps to that location (must be a JUMPDEST instruction).

    Location  Bytecode  Opcode name  
        00      36      CALLDATASIZE
        01      56      JUMP          <---
        02      FD      REVERT
        03      FD      REVERT
        04      5B      JUMPDEST
        05      00      STOP  

    Current Stack:
    4


### 3. JUMPDEST

JUMP and the value from the stack take us to JUMPDEST at location 04.

    Location  Bytecode  Opcode name  
        00      36      CALLDATASIZE
        01      56      JUMP          
        02      FD      REVERT
        03      FD      REVERT
        04      5B      JUMPDEST      <---
        05      00      STOP  

    Current Stack:
    Empty

### 4. STOP

After the JUMPDEST instruction, the PC increments by 1 and we reach the STOP (00) instruction.  


    Location  Bytecode  Opcode name  
        00      36      CALLDATASIZE
        01      56      JUMP          
        02      FD      REVERT
        03      FD      REVERT
        04      5B      JUMPDEST      
        05      00      STOP          <---

    Current Stack:
    Empty

## Solved

Second puzzle solved!

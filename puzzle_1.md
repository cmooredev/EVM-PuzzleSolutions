# Puzzle 1

    Location  Bytecode  Opcode name    
        00      34      CALLVALUE
        01      56      JUMP
        02      FD      REVERT
        03      FD      REVERT
        04      FD      REVERT
        05      FD      REVERT
        06      FD      REVERT
        07      FD      REVERT
        08      5B      JUMPDEST
        09      00      STOP

Our goal is to complete a successful transaction.  The first instruction we see is CALLVALUE (34).  This instruction takes the call value of the transaction responsible for execution and pushes it onto the stack. This is the value we will be passing in to transaction.

If you noticed, there are multiple REVERT statements.  If we reference [evm.codes](https://www.evm.codes) we can see that REVERT (FD) halts execution, reverting state changes.  We need to use JUMP and JUMPDEST to avoid REVERT instructions.  

JUMP (56) takes a counter from the stack and uses this as a byte offset to jump locations. By definition, its destination must be a JUMPDEST (5B) instruction.  Each instruction (besides PUSH opcodes, we will explore later) takes up 1 byte.  In the problem above, we can see we have a JUMP instruction at location 01 and a JUMPDEST instruction at 08. We need to provide a value to JUMP that will move us to location 08.

Following the logic above, we can enter the value 08. Now let's follow the stack.

## 1. CALLVALUE

The value 8 is passed in to the transaction and placed on the stack by CALLVALUE.

    Location  Bytecode  Opcode name    
        00      34      CALLVALUE  <---
        01      56      JUMP
        02      FD      REVERT
        03      FD      REVERT
        04      FD      REVERT
        05      FD      REVERT
        06      FD      REVERT
        07      FD      REVERT
        08      5B      JUMPDEST
        09      00      STOP

    Current Stack:
    Empty

## 2. JUMP

JUMP takes input from the stack as a location to jump to. In this case it takes 8 from the stack and jumps to that location.

    Location  Bytecode  Opcode name    
        00      34      CALLVALUE  
        01      56      JUMP       <---
        02      FD      REVERT
        03      FD      REVERT
        04      FD      REVERT
        05      FD      REVERT
        06      FD      REVERT
        07      FD      REVERT
        08      5B      JUMPDEST
        09      00      STOP

    Current Stack:
    8

## 3. JUMPDEST

JUMP and the value from the stack take us to JUMPDEST at location 08 (9th byte in the code, we start counting from 00).

    Location  Bytecode  Opcode name    
        00      34      CALLVALUE  
        01      56      JUMP      
        02      FD      REVERT
        03      FD      REVERT
        04      FD      REVERT
        05      FD      REVERT
        06      FD      REVERT
        07      FD      REVERT
        08      5B      JUMPDEST   <---
        09      00      STOP

    Current Stack:
    Empty

## 4. STOP

After JUMPDEST, the PC increments by 1 and we get moved to the next instruction, STOP (00).  This halts execution and solves the puzzle!

    Location  Bytecode  Opcode name    
        00      34      CALLVALUE  
        01      56      JUMP      
        02      FD      REVERT
        03      FD      REVERT
        04      FD      REVERT
        05      FD      REVERT
        06      FD      REVERT
        07      FD      REVERT
        08      5B      JUMPDEST
        09      00      STOP       <---

    Current Stack:
    Empty

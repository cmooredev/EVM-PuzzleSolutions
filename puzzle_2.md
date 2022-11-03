# Puzzle 2

    Location  Bytecode  Opcode name    
        00      34      CALLVALUE
        01      38      CODESIZE
        02      03      SUB
        03      56      JUMP
        04      FD      REVERT
        05      FD      REVERT
        06      5B      JUMPDEST
        07      00      STOP
        08      FD      REVERT
        09      FD      REVERT

## About the puzzle

Again, our goal is to complete a successful transaction by reaching the STOP instruction.  JUMP (56) is at location 03 and JUMPDEST (5B) is at location 06. Notice we have a few new Opcodes here.  CODESIZE (38) gets the size of the code running in the current environment.  In this puzzle, each of our opcodes occupies 1 byte.  We have a total of 10 opcodes so CODESIZE will return the value 0xA (10 in decimal).  The next instruction SUB (03) pops two inputs from the stack, and pushes the difference back onto the stack.  Order matters.

    (first item popped from stack) - (second item popped from stack)

    Stack:
    A
    B

    SUB = A - B

    New Stack:
    A-B

The next instruction is JUMP.  This instruction will pop a value from the stack and jump to that location.  The value in this case will be the result of the previous instruction, SUB. Like in the previous puzzle, we need to pass a value to the puzzle that will result in this JUMP instruction pointing to JUMPDEST at location 06.

We will pass in the value 4 and watch the stack below.


### 1. CALLVALUE

The value 4 is passed in to the transaction and placed on the stack by CALLVALUE.

    Location  Bytecode  Opcode name    
        00      34      CALLVALUE    <--- 4 passed in
        01      38      CODESIZE
        02      03      SUB
        03      56      JUMP
        04      FD      REVERT
        05      FD      REVERT
        06      5B      JUMPDEST
        07      00      STOP
        08      FD      REVERT
        09      FD      REVERT

    Current Stack:
    Empty

### 2. CODESIZE

CODESIZE gets the size of the code running in the current environment, in this case 10 bytes, or 0xA, and pushes it to the stack.

    Location  Bytecode  Opcode name    
        00      34      CALLVALUE
        01      38      CODESIZE     <---
        02      03      SUB
        03      56      JUMP
        04      FD      REVERT
        05      FD      REVERT
        06      5B      JUMPDEST
        07      00      STOP
        08      FD      REVERT
        09      FD      REVERT

    Current Stack:
    4

### 3. SUB

SUB pops the top two values from the stack and pushes the difference.  Since the stack follows Last in First out, the top value on the current stack should be 0xA (from CODESIZE), and the value below should be 4 (from CALLVALUE). 0xA - 4 = 6
0xA is 10 in decimal so this is 10 - 4 = 6. The value 6 will be pushed to the stack.

    Location  Bytecode  Opcode name    
        00      34      CALLVALUE
        01      38      CODESIZE
        02      03      SUB          <---
        03      56      JUMP
        04      FD      REVERT
        05      FD      REVERT
        06      5B      JUMPDEST
        07      00      STOP
        08      FD      REVERT
        09      FD      REVERT

    Current Stack:
    0xA ---> 10 in decimal
    4

### 4. JUMP

JUMP takes input from the stack as a location to jump to. In this case it takes 6 from the stack and jumps to that location.

    Location  Bytecode  Opcode name    
        00      34      CALLVALUE
        01      38      CODESIZE
        02      03      SUB
        03      56      JUMP         <---
        04      FD      REVERT
        05      FD      REVERT
        06      5B      JUMPDEST
        07      00      STOP
        08      FD      REVERT
        09      FD      REVERT

    Current Stack:
    6

### 4. JUMPDEST

JUMP and the value from the stack take us to JUMPDEST at location 06.

    Location  Bytecode  Opcode name    
        00      34      CALLVALUE
        01      38      CODESIZE
        02      03      SUB
        03      56      JUMP
        04      FD      REVERT
        05      FD      REVERT
        06      5B      JUMPDEST     <---
        07      00      STOP
        08      FD      REVERT
        09      FD      REVERT

    Current Stack:
    Empty

### 4. STOP

After JUMPDEST, the PC increments by 1 and we get moved to the next instruction, STOP (00).  This halts execution and solves the puzzle!

    Location  Bytecode  Opcode name    
        00      34      CALLVALUE
        01      38      CODESIZE
        02      03      SUB
        03      56      JUMP
        04      FD      REVERT
        05      FD      REVERT
        06      5B      JUMPDEST     
        07      00      STOP         <---
        08      FD      REVERT
        09      FD      REVERT

    Current Stack:
    Empty



## Solved

Second puzzle solved!

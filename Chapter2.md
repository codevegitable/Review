# Chapter 2
## Operations of the Computer Hardware
(RISC-V的汇编自己看课本)
Design Principles:
1. Simplicity favors regularity(简单的指令有利于规律性)
2. Smaller is faster(小的更快): in RISC-V there is only 32 registers, from $x_0$ to $x_{31}$, $2^{61}$ memory words.
3. Good design demands good compromises(大体思想在汇编指令中就是要保持相对位置一致)

### Memory Operands
***Data Transfer Instructions***: from register to memory or from memory to register.
To access the word or doubleword in memory, the instruction must supply the memory address. Memory is a large, one-dimension array starting at 0.
```
                    .             .
                    3           data3
                    2           data2
                    1           data1
                    0           data0
processor           address     memory
     |---------------<-------------|

the address is 64-bit, a word is 32-bit, a doubleword is 64-bit.
```

> The same as the store word in memory.

> #### Compiling Using Loading and Storing
> Assume variable h is associated with register x21 and the base address of the array A is in x22. What is the RISC-V assembly code for the C assignment statement below?
> $$
> A[12] = h + A[8]
> $$
> ```verilog
> ld x9, 64(x22)      // because there is 8 bits in a byte.
> add x9, x21, x9     // x9 = x9 + x22 -> x9 = A[8] + h
> sd x9, 96(x22)      // store the final value in A[12]
> ```

### Immediate Operands
Register x0 is set bo be value 0 (make the common case fast). For example: to present the negative number, we can just use **sub**.
```verilog
addi x9, x0, 1        // x9 = 0 + 1
sub x9, x0, x9        // x9 = 0 - x9 = -x9
```

### Signed and Unsigned Number
+ Least Significant Bit (最小端): RISC-V uses least significant bit to store binary number. It means the index $bit_0$ is the least the bit in a byte.
> Most Significant Bit is opposite to the least significant.


+ Sign and Magnitude $\rightarrow$ use just 1 and 0 to present the signed number.

+ 2's Complement $\rightarrow \ (x_{63} \times -2^{63}) + (x_{62} \times 2^{62}) + (x_{61} \times 2^{61}) + ... + (x_{0} \times 2^{0})$ 

> Sign Extension: Fill most significant with sign bit. For example: $16'h0002 \rightarrow 64'h0000000000000002$, using sign bit '0' to complete sign extension.

+ 1's Complement $\rightarrow$ just reverse the binary number use the sign bit.

## Presenting Instructions in the Computer
+ R-type format: 

    |funct7|rs2 <br> second register source operand|rs1 <br> first register source operand|funct3|rd <br> gets the result of the operation|opcode|
    |:----:|:-:|:-:|:----:|:-:|:----:|
    |7 bits|5 bits|5 bits|3 bits|5 bits|7 bits|

+ I-type format:

    |immediate|rs1|funct3|rd <br> gets the result of the operation|opcode|
    |:----:|:-:|:----:|:-:|:----:|
    |12 bits|5 bits|3 bits|5 bits|7 bits|

+ S-type format:

    |immediate[11:5]|rs2|rs1|funct3|immediate[4:0]|opcode|
    |:----:|:-:|:-:|:----:|:-:|:----:|
    |7 bits|5 bits|5 bits|3 bits|5 bits|7 bits|

> **Exercise**: If x10 has the base of the array A and x21 corresponds to h, the assignment statement:
> $$
> A[30] = h + A[30] + 1;
> $$
> is compiled to:
> ```verilog
> ld x9, 240(x10)
> add x9, x21, x9
> addi x9, x9, 1
> sd x9 240(x10)
> ```

## Logical Operations
+ shift: shift left logical immediate and shift right logical immediate.
    > only use lower 6 bits in 64-bit register and 5 bits in 32-bit register. We can call the extra bits in immediate funct6.

+ and, or, not, xor

## Instructions for Making Decisions
+ branch if equal (beq): 
    ```verilog
    beq rs1, rs2, L           // go to the statement labeled L if rs1 = rs2
    ```

+ branch if not equal (bne):
    ```verilog
    bne rs1, rs2, L           // go to the statement labeled L if rs1 ≠ rs2
    ```

    > **Exercise**: f through j correspond to the five registers x19 through x23, what is the compiled RISC-V code for this C if statement:
    > ```C
    > if (i == j) f = g + h; else f = g - h;
    > ```
    >
    > ```verilog
    > bne x22, x23, Else
    > add x19, x20, x21
    > beq x0, x0, EXIT         // avoid to excute the sub instruction
    > Else: sub x19, x20, x21
    > EXIT:
    > ```

+ blt: branch if rs1 is less than rs2 (rs1 and rs2 are 2's completement).

+ bge: branch if rs1 is greater than or equal to rs2 (rs1 and rs2 are 2's completement).

+ bltu: less than (unsigned)

+ bgeu: less than (unsigned)

> + Bounds Check Shortcut: check if out of the bound: x22 >= x11 || x22 < 0, while x22 = i and x11 = size (a negative number often has more bit 1s in the most significant bit). 
>
>   ```verilog
>   bgeu x22, x11, IndexOutofBound      // x22 >= x11 || x22 < 0 branch to Index to Out of Bound
>   ```
>
> + Case / Switch Statement: uses branch. Alternatives may be more efficiently encoded as a table of address of alternative instruction sequences, called a branch address table or branch table. So the program is just an array of double-words containing address that correspond to labels in the code. $\rightarrow$ jalr
>+ loops: 
> **Exercise**: i and k correspond to registers x22 and x24 and the base of the array save is x25. 
>   ```C
>   while(save[i] != k) i += 1;
>   ```
> 
>   ```verilog
>   Loop: slli x10, x22, 3    // acculate the x10 = i * 8
>         add x10, x25, x10
>         ld x9, 0(x10)
>         beq x9, x24, Exit
>         addi x22, x22, 1
>         bne x9, x24, Loop
>   Exit:
>   ```

## Supporting Procedures in Computer Hardware
+ jal: jump and link instruction. 
    ```verilog
    jal x1, ProcedureAddress      // jump to Procedure Address and write return address to x1.
    ```

+ jalr: jump and link register instruction.
    ```verilog
    jalr x0, 0(x1)
    ```
    > branches to the desired register x1; then the calling program (caller), puts the parameter values in x10-x17 and uses ```jal x1, X``` to branch to procedure X (calle). The calle then performs the calculations, places the results in the same parameter registers, and returns control to the caller using ```jalr x0, 0(x1)```

    > Mention that the register x1 often stores the return address (PC + 4, the next instruction) and registers x10-x17 often store the instruction parameters (PC + offset, jump to the instruction).
    
    > unconditionally jump: ```jal x0, Label```, x0 is hard-wired to 0 so it doesn't store the return address (stop the program / loop).

+ stack: use the register x2 (sp) to be the stack pointer. stack grows from higher addresses to lower addresses.
    > Exercise1: g, h, i, j correspond to the argument register x10, x11, x12 and x13, and f corresponds to x20.
    > ```C
    > long long int leaf_example (long long int g, long long int h, long long int i, long long int j)
    > {
    >     long long int f;
    >     f = (g + h) - (i + j);
    >     return f;
    > }
    > ```
    > 
    > ```verilog
    > leaf_example: addi sp, sp, -24
    >                 sd x5, 16(sp)
    >                 sd x6, 8(sp)
    >                 sd x20, 0(sp)
    >                add x5, x10, x11
    >                add x6, x12, x13
    >                sub x20, x5, x6
    >               addi x10, x20, 0
    >                 ld x20, 0(sp)
    >                 ld x6, 8(sp)
    >                 ld x5, 16(sp)
    >               addi sp, sp, 24
    >               jalr x0, 0(x1)
    > ```

    > Exercise2: n corresponds to the argument register x10, and return to x10.
    > ```C
    > long long int fact (long long int n)
    > {
    >     if (n < 1) return (1);
    >     else return (n * fact (n - 1))
    > } 
    > ```
    > 
    > ```verilog 
    > fact: addi sp, sp, -16
    >         sd x1, 8(sp)
    >         sd x10, 0(sp)
    > addi x5, x10, -1
    >  bge x5, x0, L1
    > addi x10, x0, 1
    > addi sp, sp, 16
    > jalr x0, 0(x1)
    > L1: addi x10, x10, -1
    >      jal x1, fact
    > addi x6, x10, 0
    >   ld x10, 0(sp)
    >   ld x1, 8(sp)
    > addi sp, sp, 16
    > mul x10, x10, x6
    > jalr x0, 0(x1)
    > ```

|preserved|not preserved|
|:-------:|:-----------:|
|Saved register: x8-x9, x18-x27|Temporary regsiter: x5-x7, x28-x31|
|Stack pointer register: x2 (sp)|Argument / result register: x10-x17|
|Frame pointer: x8 (fp)||
|Return address: x1 (ra)||
|Stack above the stack pointer|Stack below the stack pointer|


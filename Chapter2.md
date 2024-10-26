<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
# Chapter 2
## Operations of the Computer Hardware
(RISC-V的汇编自己看课本)
Design Principles:
1. Simplicity favors regularity(简单的指令有利于规律性)
2. Smaller is faster(小的更快): in RISC-V there is only 32 registers, from $x_0$ to $x_{31}$.
3. 

### Memory Operands
data transfer instructions: from register to memory or from memory to register.
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
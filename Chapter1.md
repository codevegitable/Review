<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
# Chapter 1
## Eight Great Ideas in Computer Architecture
1. ```Design for Moore's Law```: (设计跟随摩尔定律)
   + integrated circuit resources double every 18-24 months.
   + computer architects must anticipate where the technology will be when the design finishes rather than design for where it starts.(计算机设计师必须预见到设计完成时的技术状况，而不是设计开始时的技术状况。)
2. ```Use Abstraction to Simpilfy Design```: (使用抽象来简化设计)
   + characterize the design at different levels of representation.(层次化、模块化设计)
3. ```Make the Common Case Fast```: (加速大概率事件)
   + making the common case fast will tend to enhance performance better than optimizing the rare case.
4. ```Performance via Parallelism```: (通过并行提高性能)
   + (课本里面说了句废话)
5. ```Performance via Pipelining```: (通过流水线提高性能)
   + every process is handled at the same time rather than waiting for another one finished.
   + hoping that the time of every process is relatively even.
   (计算机中的多线程和多进程)
6. ```Performance via Prediction```: (通过预测提高性能)
   + faster on average to guess and start working than wait until you know for sure.
   + accelerate while predicting successfully, and correcting cost is not so high while predicting mistakenly.
7. ```Hierarchy of Memories```: (存储器层次)
   + fasted, smallest, and the most expensive memory per bit at the top of the hierarchy.
   + slowest, biggest, and the cheapest memory per bit at the bottom of the hierarchy.
   + Disk/Tape -> Main Memory(DRAM) -> L2-Cache(SRAM) -> L1-Cache(On-Chip) -> Registers
8. ```Dependability via Redundancy```: (通过冗余提高可靠性)
   + make system dependable, when one component collapse down, others can still run.

## Below Your Program (Abstraction)
a complex application -> several layers interpret and translate into simple instructions.
```
Application software -> System software -> Hardware 
                               |
           operating system   and    compiler
```

> + operating system:
>   + handling basic I/O.
>   + allocating storage and memory.
>   + providing for protected sharing of the computer among multiple applications. 
> + compilers: the translation of a program written in high-level language into instructions computer can execute.
>   + high-level language program ->(Compiler) assembly language program ->(Assembler) binary machine language

## Under the Cover (The Components of the Computer)
冯·诺伊曼架构

> two key components
+ input device
+ output device
> CPU
+ control: send the controlling signal
+ datapath: process the data
> memory
+ memory: store the program and data, also run the program; cache memory(SRAM): buffer for the DRAM.

基本概念：
> instructions set architecture: interface between hardware and low-level software. 
> application binary instruction: interface between instruction set and operating system.

## Technologies for Building Processors and Memory
integrated circuit (IC): dozens to hundreds of transistors into a single chip

VLSI circuit (very large-scale integrated circuit): tremendous increasing in transistor.

dies/chips: the good ones after wafer; defects: the bad ones.

> The cost of an integrated circuit can be expressed in three simple equations:
> + $\text{cost per die} = \frac{\text{cost per wafer}}{\text{dies per wafer} \times yield}$
> + $\text{dies per wafer} \approx \frac{\text{wafer area}}{\text{die area}}$
> + $yield = \frac{1}{(1 + (\text{defects per area} \times \text{die area}/2))^2}$ (exponent related to the number og critical processing steps)<br>
> yield is the result of observation, not calculation.

## Performance
execution time/ response time: the time between start and completion of a task.

throughput / bandwidth: the total amount of work done in a given time.

Performance and Execution Time:
+ $Performance_X = \frac{1}{\text{Execution time}_X}$
+ X is n times faster than Y: $\frac{Performance_X}{Performance_Y} = n = \frac{\text{Execution time}_Y}{\text{Execution time}_X}$

CPU Time:
+ clock cycles: when events take place in hardware.
+ $\text{CPU execution time for a program / CPU time} = \text{CPU clock cycles for a program} \times \text{Clock cycle time} = \frac{\text{CPU clock cycles for a program}}{\text{Clock rate}}$
+ $\text{CPU clock cycles} = \text{Instructions for a program} \times \text{Average clock cycles per instruction(CPI)}$
+ summary: $\text{CPU time} = \text{Instructions count} \times CPI \times Clock cycle time = \frac{\text{Instructions count} \times CPI}{\text{Clock rate}}$
+ (in the example): $\text{CPU clock cycles} = \sum_{i=1}^n (CPI_i \times C_i)$, when finish the calculation, use the $CPI_i = \frac{\text{CPU clock cycles}_i}{Instruction count_i}$
> The only reliable and complete measure of computer performance is time.<br>

| Hardware or software component |Affects what |
|:------------------------------:|:-----------:|
|Algorithm|Instruction count, CPI|
|Programming language|Instruction count, CPI|
|Compiler|Instruction count, CPI|
|Instruction set architecture|Instruction count, CPI, clock rate|

## The Power Wall 
The clock rate and the power increased together rapidly for decades, and flatten off recently.(the power meet the limit)

The dynamic energy(for CMOS) depends on the capacitive loading of each transistor and the voltage applied:
> $Energy \propto \text{Capacitive loading} \times Voltage^2$ (a transistor switches from 1-0-1 or 0-1-0)

The energy for single transistor is:
> $Energy \propto 1/2 \times \text{Capacitive loading} \times Voltage^2$

The power required per transistor:
> $Power \propto 1/2 \times \text{Capacitive loading} \times Voltage^2 \times \text{Frequency switched}$

## Pitfalls and Fallacies
_Pitfall: Expecting the improvement of one aspect of a computer to increase overall performance by an amount proportional to the size of the improvement._<br>
$T_{improvement} = \frac{T_{affected}}{\text{Amount of improvement}} + T_{unaffected}$
> for example: $100\ seconds = \frac{\text{80 seconds}}{n} + \text{20 seconds}$. <br>
> if CPU time is 5 times faster, then:<br>
> $20\ seconds = \frac{\text{80 seconds}}{n} + \text{20 seconds}$, $0 = \frac{\text{80 seconds}}{n}$,<br> 
> obviously wrong.

_Fallacy: Computers at low utilization use little power._

_Fallacy: Design for performance and design for energy efficiency are unrelated goals._

_Pitfall: Using a subset of the performance equation as a performance metric._<br>
> $MIPS\text{(millions of instructions per second)} = \frac{\text{Instruction count}}{\text{Execution time} \times 10^6} = \frac{\text{Clock rate}}{CPI \times 10^6}$<br>
> mention that MIPS doesn't take into account the capabilities of the instructions. 


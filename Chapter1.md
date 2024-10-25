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
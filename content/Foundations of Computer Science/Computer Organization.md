---
title: "Computer Organization"
date: 2018-12-09T22:00:00+08:00
categories: [Foundations of Computer Science]
tags: [CPU, GPU, Memory, Register, Cache, SSD, HDD, CD, DVD, I/O, DMA, CISC, RISC]
toc: true
math: false
---

## CPU

- Control Unit
    - A logic circuit that controls the CPU in respond of the given instructions.
- Arithmetic Logic Unit
    - logic operations, shift operations, arithmetic operations
- Registers(basic C programming knowledge required)
    - word size: 64 bits vs 32 bits
    - User accessible:
        - General purpose register: stores data or addresses
        - Data register: stores data
        - Address register: stores address
        - Stack pointer: address of the top of the current program stack
        - Base pointer: address of the starting position of the current stack frame
        - Program counter: address of the next instruction
        - Constant register: e.g. 0, pi
    - Internal:
        - Instruction register: current instruction being executed
        - Memory address register: address of the data to be fetched/stored
        - Memory data register: data to be fetched/stored
        - ![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d3/Call_stack_layout.svg/342px-Call_stack_layout.svg.png)
    - I/O

## Memory
- Addressing
    - Smallest addressing unit in convention: byte
    - Question:
        - How many bits do you need in order to map 64 MB of memory?
        - How much memory can you map with 32 bits?
- RAM: volatile
- ROM: contents written by manufacturer, can't be erased
- PROM: contents can be write by user once, can't be erased
- EPROM: a PROM that can be erased with UV light, requires physical removal
- EEPROM: a PROM that can be erased with electronic impulses
- Speed: RAM >> ROM

### Cache

Acting as a memory device in between registers and main memory.
- Size: x KB ~ x MB
- Very efficient: frequently accessed data are usually in the minority.
- Procedure:
    1. CPU needs specific data
    2. Check cache
        1. Cache hit: Jump to 3
        2. Cache miss:
            1. Copy data from memory to cache
    3. Get data from cache

## Input/Output Devices

### Nonstorage Devices

- Monitor
- Speaker
- Keyboard
- Touchpad
- Mouse
- Microphone
- Camera
- Printer

### Storage Devices

#### Integrated Circuit

- SSD

#### Magnetic

- [HDD](https://hackmd.io/nV0oHCiDR-uZ63IXOFrlAg?both#Storage)
    - Random Access
    - Disks
    - Tracks
    - Sectors
    ![](https://gateoverflow.in/?qa=blob&qa_blobid=2012281201284991156)
    - Performance:
        - Slow, compared to SSD
        - Rotational speed
        - Seek time
        - Seeking algorithms
            - FCFS vs SSTF
            - ![](https://i.imgur.com/LFB2qPT.png)
            - ![](https://i.imgur.com/US0o6e2.png)
- Magnetic Tape
    - Sequential Access

![](https://img.bhs4.com/17/5/175512094F06E00D9869A8E36974F8DFCECFCC13_large.jpg)

![](https://www.sfu.ca/sonic-studio-webdav/handbook/Graphics/Emulsion2.gif)

#### Optical

- CD
    ![](https://preview.redd.it/ginx3f15wb221.jpg?width=640&crop=smart&auto=webp&s=753e2c90a817fe03d348638de39310145f0beb25)

    - CD-ROM
        - [Production](https://drive.google.com/file/d/1g3up957qY22A_bWUMCnc1h-2RrsJ3dyX/view?usp=sharing)
    - CD-R
        - Simulated pits/lands
        - Aluminum
    - CD-RW
        - Simulated pits/lands
        - Gold
    - Size: 650MB ~ 900MB

- DVD
    - Red laser

| Specification | Capacity |
| ------------- | -------- |
| Single-sided, single-layer | 4.7 GB |
| Single-sided, dual-layer | 8.5 GB |
| Double-sided, single-layer | 9.4 GB |
| Double-sided, dual-layer | 17 GB |

![](http://kopyrite.com/wp-content/uploads/2013/02/DVD-1.gif)

![](https://upload.wikimedia.org/wikipedia/commons/a/ad/Comparison_CD_DVD_HDDVD_BD.svg)

## Subsystem Interconnection

### CPU and Memory Connection
- Data bus
    - native support of 64 bit integer operations
    - m bits data needs: m bits
- Address bus
    - 32 bit limit: 4 GB
    - 64 bit
    - 2^m^ addresses needs: m bits
- Control bus
    - 2^m^ control commands needs: m bits

### I/O Connections

- [SCSI](https://drive.google.com/file/d/1X4WUko6KoYtDg1r8TOy1BaDp_mAUYIAD/view?usp=sharing)
- [Firewire](https://drive.google.com/file/d/194PrwJe8Vb60w91gFM-mUpVZw1lANRFB/view?usp=sharing)
- [USB](https://drive.google.com/file/d/1uobBTX00PP8EkQl7Q0Ca1q7S-t_iGnV9/view?usp=sharing)
    - Tree-like structure
    - Hot pluggable
- VGA
- HDMI

### Port-mapped I/O (PMIO)
- == Isolated I/O
- Dedicated address space
- Dedicated instruction set
- Simple operations

### Memory-mapped I/O (MMIO)
- Maps I/O to memory(combined address space)

### Interrupt
- Emergency

## Program Execution

### Instructions

1. Fetch
2. Decode
3. Execute

### Input/Output

#### Programmed I/O

- CPU direct

1. Issue command
2. Constantly check availability
3. Transfer

#### Interrupt I/O

- CPU direct

1. Device interruption
2. Transfer

#### Direct Memory Access

If there's no DMA, CPU would be working on I/Os mostly!

- CPU indirect

1. CPU issues command(what to do, starting address, number of bytes)
2. DMA preperation
3. DMA is ready and interrupts CPU
4. CPU checks and gives bus permission to DMA
5. DMA transfers data
6. DMA finishes its job and intrrupts CPU

## Instruction Set Architecture(ISA)

### Complex Instruction Set Computer(CISC)

- Large set of complex and specialized instructions
- Complex instructions are translated into simple ones (microoperations)before executing.
- Architecture: x86, x86-64, Zen, Zen+, Zen2
- Products
    - Intel: Core, Xeon...
    - AMD: Ryzen...
- Target: Desktops, servers, etc.

### Reduced Instruction Set Computer(RISC)

- Small set of simple and general instructions
- Usually: uniform instruction format, using single word with the opcode in the same bit positions for simpler decoding
- Architectures: ARM, MIPS, RISC-V
- Products: Cortex, Snapdragon
- Target: Smartphones, IOT devices...

### Pipelining

![](https://www.cise.ufl.edu/~mssz/CompOrg/Figure5.3-MipsPipeline.gif)

### Parallel Processing

- SISD: non-parallel
- SIMD
- MISD?
- MIMD: Modern PCs

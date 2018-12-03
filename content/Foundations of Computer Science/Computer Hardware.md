---
title: "Computer Hardware"
date: 2018-11-29T22:30:00+08:00
categories: [Foundations of Computer Science]
tags: [CPU, GPU, Memory, SSD, HDD]
toc: true
math: false
---

- CPU
    - CU
    - ALU
    - Data registers
    - I/O
    ![](https://qph.fs.quoracdn.net/main-qimg-9be4168022f2e8a08981cbd1134651c3)
    - Architecture: x86, x86-64, ARM, RISC-V, MIPS
    - Vendors: Intel, AMD, Apple
- Storage
    - Register
        - User accessible
            - General purpose register: stores data or addresses
            - Data register: stores data
            - Address register: stores address
            - Stack pointer: address of the top of the current program stack
            - Base pointer: address of the starting position of the current stack frame
            - Program counter: address of the next instruction
            - Constant register: e.g. 0, pi
        - Internal registers
            - Instruction register: current instruction being executed
            - Memory address register: address of the data to be fetched/stored
            - Memory data register: data to be fetched/stored
    - Cache
        - L1 ~ L3
        - Motivation: Fetching data from main memory takes a lot of time.
    - (Main) memory = RAM
        - What is RAM
        - Volatile
            - SRAM
                - Fast
            - DRAM
                - Slow
            - SDRAM
                - DDR1 ~ DDR4
            - Speed: 2133, 2400, 1600MHz
        - Non-volatile(NVRAM)
            - Flash
    - Storage
        - SSD
            - Fast, short lived, integrated circuit
        - HDD
            - Slow, long lived, magnetic storage
            - Weak against magnetic fields and vibration
            - speed: 5400rpm, 7200rpm
            - ![](https://www.cs.uic.edu/~jbell/CourseNotes/OperatingSystems/images/Chapter10/10_01_DiskMechanism.jpg)
        - SSD vs HDD?
        - Interfaces
            1. Drivers: NVME vs Intel RAID vs AHCI vs IDE
            2. PCIE vs SATA
            3. M.2(right) vs mSATA(left)
            ![mSATA vs M.2](https://upload.wikimedia.org/wikipedia/commons/c/c5/M.2_and_mSATA_SSDs_comparison.jpg)

            ![](https://cclgroupltd.com/wp-content/uploads/2018/07/03_SATA_Express_interface.png)
            4. Valid combinations:
                - PCIE, NVME
                - PCIE, AHCI
                - SATA, AHCI
    - Speed
    - ![](https://images.bit-tech.net/content_images/2007/11/the_secrets_of_pc_memory_part_1/hei.png)

- Bus
    - Internal
        - SATA
        - PCIE
    - External
        - USB Type A(2.0, 3.0, 3.1gen1), C(3.1gen2, 3.2, thunderbolt)
        - HDMI
        - VGA
        - SD card
        - Ethernet
        - 3.5mm Audio Jack
- GPU
    - Purpose: large scale, highly paralleled simple tasks
    - Vendors: Intel, Nvidia, AMD
    -
| CPU |  GPU |
|:-:| :----------: |
| sophisticated tasks, precision | highly paralleled simple tasks|
| small amount of calculations per time | large anoumt of calculations per time |
| complilation, virtual machine emulation, complex control flows and branching | Graphics, neural networks, crypto mining, password/hash cracking |
- Motherboard
![](https://i.imgur.com/tD2o5a5.png)
    - North birdge is vanishing over time.
- Peripherals
    - Keyboard
    - Mouse
    - Touchpad
    - Screen
    - Printer

## Questions
1. What is your laptop specifications?
2. Find your ideal laptop.
3. What are the following components? CPU, GPU, SSD, HDD, DDR3 RAM, register, cache.

..
   Copyright (c) 2025 Thales DIS France SAS
   Licensed under the Solderpad Hardware Licence, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   SPDX-License-Identifier: Apache-2.0 WITH SHL-2.0
   You may obtain a copy of the License at https://solderpad.org/licenses/

   Original Author: André Sintzoff - Thales DIS

CV32A60X Design Document
========================

Introduction
------------

The CV32A60X is a 32-bit application class RISC-V CPU core based on the CVA6.
It is a 6-stage, single-issue, in-order processor implementing the RV32 instruction set.
The CV32A60X targets machine-mode-only embedded and IoT applications with a compact
footprint and support for the CORE-V eXtension Interface (CVXIF) for custom instructions.

Features
--------

Instruction Set Architecture
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The CV32A60X implements the following RISC-V ISA extensions:

- **RV32I**: 32-bit base integer instruction set
- **M**: Integer multiplication and division
- **C**: Compressed 16-bit instructions (reduces code size)
- **B**: Bit-manipulation (Zba, Zbb, Zbc, Zbs sub-extensions)
- **Zcb**: Additional 16-bit compressed instructions (code-size reduction)
- **Zicsr**: Control and Status Register instructions

Pipeline
~~~~~~~~

The CV32A60X implements a 6-stage pipeline:

1. Instruction Fetch (IF)
2. Instruction Decode (ID)
3. Issue (IS)
4. Execute (EX)
5. Memory Access (MEM)
6. Write Back (WB) / Commit

The core is single-issue and in-order. It does not support superscalar execution.

Privilege Modes
~~~~~~~~~~~~~~~

The CV32A60X implements **machine mode (M-mode) only**.
Supervisor mode (S-mode) and user mode (U-mode) are not supported.
As a result, no MMU or virtual memory support is required.

Memory Management
~~~~~~~~~~~~~~~~~

The CV32A60X does not include a Memory Management Unit (MMU).
Physical memory protection (PMP) is also not implemented.

The core includes:

- **Instruction cache**: 2 KiB, 2-way set-associative, 128-bit line width
- **Data cache**: 2 KiB, 2-way set-associative, 128-bit line width (write-through)

CORE-V eXtension Interface (CVXIF)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The CV32A60X implements the CORE-V eXtension Interface (CVXIF), allowing
external coprocessors to extend the instruction set with custom operations.
This enables domain-specific acceleration without modifying the core.

Branch Prediction
~~~~~~~~~~~~~~~~~

The CV32A60X includes a Branch History Table (BHT) with:

- **BHT entries**: 32
- **BHT history length**: 3 bits
- **Return Address Stack (RAS) depth**: 2
- No Branch Target Buffer (BTB)

AXI Interface
~~~~~~~~~~~~~

The CV32A60X uses an AXI4 bus interface with the following parameters:

- **Address width**: 64 bits
- **Data width**: 64 bits
- **ID width**: 4 bits
- **User width**: 32 bits

Configuration Parameters
------------------------

The following table summarizes the key configuration parameters of the CV32A60X.

.. list-table:: CV32A60X Configuration Parameters
   :widths: 30 15 55
   :header-rows: 1

   * - Parameter
     - Value
     - Description
   * - XLEN
     - 32
     - Register width (32-bit ISA)
   * - VLEN
     - 32
     - Virtual address size in bits
   * - NrCommitPorts
     - 1
     - Number of commit ports (single-issue)
   * - NrScoreboardEntries
     - 4
     - Number of scoreboard entries
   * - IcacheByteSize
     - 2048
     - Instruction cache size in bytes
   * - IcacheSetAssoc
     - 2
     - Instruction cache associativity
   * - IcacheLineWidth
     - 128
     - Instruction cache line width in bits
   * - DcacheByteSize
     - 2028
     - Data cache size in bytes
   * - DcacheSetAssoc
     - 2
     - Data cache associativity
   * - DcacheLineWidth
     - 128
     - Data cache line width in bits
   * - DCacheType
     - Write-through (WT)
     - Data cache write policy
   * - BHTEntries
     - 32
     - Branch History Table entries
   * - BHTHist
     - 3
     - Branch History Table history bits
   * - RASDepth
     - 2
     - Return Address Stack depth
   * - NrLoadBufEntries
     - 2
     - Number of load buffer entries
   * - MmuPresent
     - 0
     - No Memory Management Unit
   * - NrPMPEntries
     - 0
     - No Physical Memory Protection entries
   * - DebugEn
     - 0
     - Debug module disabled
   * - CvxifEn
     - 1
     - CORE-V eXtension Interface enabled

ISA Extensions
--------------

.. list-table:: Supported ISA Extensions
   :widths: 15 10 75
   :header-rows: 1

   * - Extension
     - Enabled
     - Description
   * - RV32I
     - Yes
     - 32-bit base integer instruction set
   * - M
     - Yes
     - Integer multiplication and division
   * - C
     - Yes
     - 16-bit compressed instructions
   * - B (Zba/Zbb/Zbc/Zbs)
     - Yes
     - Bit-manipulation extensions
   * - Zcb
     - Yes
     - Additional compressed instructions
   * - Zicsr
     - Yes
     - Control and Status Register instructions
   * - F
     - No
     - Single-precision floating point (not supported)
   * - D
     - No
     - Double-precision floating point (not supported)
   * - A
     - No
     - Atomic instructions (not supported)
   * - V
     - No
     - Vector extension (not supported)
   * - S-mode
     - No
     - Supervisor mode (not supported)
   * - U-mode
     - No
     - User mode (not supported)

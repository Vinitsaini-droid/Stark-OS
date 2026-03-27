Stark OS Architecture: Memory Model \& Consistency



1\. Objective

To define a formally verifiable interaction model between the Stark OS kernel and modern hardware memory hierarchies. This document serves as the technical mandate for all memory-related subsystems, prioritizing safety and scalability over legacy compatibility.



2\. The Multi-kernel Paradigm

Stark OS rejects the "global mutable state" found in monolithic kernels.

\* Non-Shared State: Each CPU core runs its own "monitor" or local kernel instance. Cores share no kernel memory by default.

\* Consistency via Messages: System-wide state changes (like global page table updates) are managed through explicit message passing and two-phase commit protocols rather than shared-memory locking.

\* Benefit: Eliminates "Stop the World" events and cache-line bouncing on high-core-count systems.



3\. Memory Ordering \& Consistency

Stark OS targets weakly ordered architectures (ARMv8-A, RISC-V) as primary execution environments.

\* Weak Ordering Assumption: The system assumes the hardware may reorder any memory operation that does not have an explicit dependency.

\* Explicit Synchronization: Synchronous consistency must be explicitly restored using memory barriers (fences) and C11-style Acquire/Release semantics.

\* Formal Relation: Concurrent execution outcomes are governed by the relations of Preserved Program Order and Global Memory Order.



4\. Hardware-Assisted Isolation

Safety is enforced at the hardware level to eliminate unbounded memory access and spatial/temporal memory errors.

&#x20;  \* Memory Tagging: Every 16-byte granule of memory is assigned a unique hardware tag (utilizing ARM MTE or RISC-V Zimt).

&#x20;  \* Pointer Validation: Pointers must store matching tags in their upper bits (Top-Byte Ignore). Mismatches result in immediate hardware-level faults.

&#x20;  \* Result: Effectively eliminates buffer overflows and use-after-free vulnerabilities in kernel space.



5\. Topology \& Performance (NUMA)

To achieve "million-core scalability," the system is designed to be topology-aware from the first boot.

&#x20;  \* NUMA-Aware Allocation: The local monitor on each core prioritizes physical DRAM nodes closest to that core (local affinity).

&#x20;  \* Directory-Based Coherency: Communication is point-to-point to avoid the bandwidth bottlenecks of traditional "snoopy" bus protocols.



6\. IPC Memory Mechanics

Inter-process communication (IPC) is the primary method of data exchange.

&#x20;  \* Shared Ring Buffers: High-frequency transfers use mapped physical regions for zero-copy data exchange.

&#x20;  \* Page-Table Displacement: Large data transfers are handled by remapping pages between address spaces, involving only TLB flushes and no data copying.

&#x20;  \* Capability Authentication: All memory regions are identified and accessed via unforgeable capabilities, ensuring a process can never "name" memory it does not own.


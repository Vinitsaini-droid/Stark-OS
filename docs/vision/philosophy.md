\# The Stark Philosophy



This document defines the non-negotiable engineering principles of Stark OS. Every line of code added to this repository must align with these Axioms.



\## I. The Stark of Correctness

\*\*"Correctness Before Features."\*\*

In systems programming, technical debt is often fatal. We do not accept "hacks" to achieve quick milestones. We prefer a system that does 3 things perfectly and provably over a system that does 30 things unreliably.



\## II. The Legacy Rule

\*\*"Build for the next 30 years, not the last 30."\*\*

Stark OS purposefully ignores legacy hardware. We do not support BIOS, VGA text mode, or 32-bit architectures. By restricting our scope to modern UEFI-based, 64-bit systems with advanced MMUs, we keep the kernel lean and optimized for the future of silicon.



\## III. Explicit Concurrency

\*\*"Share nothing, communicate by message."\*\*

Shared mutable state is the enemy of scalability. Stark treats every CPU core as an isolated node. Synchronization is achieved through explicit Inter-Process Communication (IPC) and message passing, bypassing the "lock-contention" bottlenecks that plague monolithic kernels.



\## IV. User Agency \& Isolation

\*\*"The Kernel is a Librarian, not a Dictator."\*\*

The kernel's only job is to manage resources and enforce boundaries. Drivers, filesystems, and networking stacks belong in user space. This ensures that a failure in a disk driver does not bring down the entire system, and allows users to swap core components without rebooting.



\## V. Zero-Trust Architecture

\*\*"Isolation is the Baseline."\*\*

We assume that components will be compromised. We use capability-based security to ensure that a breach in one subsystem provides zero leverage over the rest of the system.


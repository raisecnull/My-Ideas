# What if shell and I/O drivers are perfectly fine if your kernel panics?

If the kernel panics, it would result in a BSOD in windows or kernel panic on linux. But I wonder, what if I create a Multi-kernel OS that separates the shell and driver of keyboard where even if your kernel panics, its not completely freezed?

My idea is to create 2 separate kernels of an OS in a way that the kernels can detect each other, process concurrently and detect if the other kernel panics.

- But the problem here is if I create a "kernel A" to work perfectly fine when "kernel B"(main kernel) panics or crashed, what if "kernel A" is the one that crashed?, well I guess I need to build something that detects each other.
- So the final idea is "kernel A" will still work even "Kernel B" crashed but "kernel A" can still crash so need some kind of communication between them.

**from thorough research, though this idea looks simple on the paper, but highly complex because:**
- kernel A and kernel B are completely blind to each other's memory, kernel A doesnt know that a memory belonds to kernel B and might modify it or corrupt it. We need to enforce a strict barrier between their memory.
- - race conditions, because its a concurrent multiprocessing kernel, its prone to a lot of race conditions that can raise severe security issues.
  - I need to program the `Advanced Programmable Interrupt Controller (APIC)` for the motherboards peace of mind.
  - Because core 0 are the one awake, we need to wake up other core during start up
 
    ![](https://y.getyarn.io/99003235-b5cd-4883-a61a-58a09b3e15a4_text.gif)

1. Is there a clean split of user-vs-kernel mode operations in an FPGA driver? (Nvidia analogue -
   JITing in user mode and setting GPU registers via MMIO and passing JITed code to the GPU via
   DMA)?
2. Since the RTL and driver are developed side by side, is there a bit of a dance needed to handle
   the difference in iteration speed - compiling the driver vs `synthesis+place_and_route` for the
   FPGA?
3. What does driver debugging/tuning look like? Since it is a niche area that sits behind closely
   guarded IP, where off-the-shelf debug tools may not apply, is it part of the driver developers
   job to develop debug tools?
4. For completion signalling, where do you land on interrupts vs. busy-poll? In CUDA you can choose
   between blocking and spin-waiting on a stream; here I'd assume a hot core just spins on a memory
   flag the card writes — is the kernel ever in the path at all on the critical road?
5. How do you handle PCIe ordering and the memory-model guarantees between card and host? On the GPU
   side a lot of that is hidden behind the runtime; here it sounds like you own the fences and the
   "has the DMA actually landed" question yourself. How much of the hard bugs live there?
6. How does the driver expose the card to the C++ trading layer above it — a thin MMIO/DMA shim, or a
   richer API? I'm curious how much policy lives in the driver vs. the application, compared to the
   thick abstraction CUDA hands you.
7. When the RTL team changes a register layout or moves a queue, how is that contract managed so the
   driver and hardware don't silently drift apart? Is there a shared source of truth (generated
   headers, an address map), or is it more manual than that?

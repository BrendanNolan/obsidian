1. Is there a clean split of user-vs-kernel mode operations in an FPGA driver? (Nvidia analogue -
   JITing in user mode and setting GPU registers via MMIO and passing JITed code to the GPU via
   DMA)?
2. Since the RTL and driver are developed side by side, is there a bit of a dance needed to handle
   the difference in iteration speed - compiling the driver vs `synthesis+place_and_route` for the
   FPGA?
3. At a lower level than the previous question: how much can be automated in the task of keeping the
   RTL and driver teams in sync while register layouts change, queues move, etc.?
4. What does driver debugging/tuning look like? Since it is a niche area that sits behind closely
   guarded IP, where off-the-shelf debug tools may not apply, is it part of the driver developers
   job to develop debug tools?
5. For completion signalling, where do you land on interrupts vs. busy-poll?
6. How does the driver expose the card to the C++ trading layer above it — a thin MMIO/DMA shim, or
   a richer API? I'm curious how much policy lives in the driver vs. the application.

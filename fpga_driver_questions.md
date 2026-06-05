1. Is there a clean split of user-vs-kernel mode operations in an FPGA driver? (Nvidia analogue -
   JITing in user mode and setting GPU registers via MMIO and passing JITed code to the GPU via
   DMA)?
2. Since the RTL and driver are developed side by side, is there a bit of a dance needed to handle
   the difference in iteration speed - compiling the driver vs `synthesis+place_and_route` for the
   FPGA?
3. What does driver debugging/tuning look like? Since it is a niche area that sits behind closely
   guarded IP, where off-the-shelf debug tools may not apply, is it part of the driver developers
   job to develop debug tools?

1. Does an FPGA hot path ever need to access memory from DRAM? Does the low-level control offered by
   an FPGA allow you to escape the jitter that is expected when a general-purpose machine needs to
   access DRAM and may clash with the DRAM refresh?
2. Do you guys need to worry about parasitic effects on the FPGA, or do the
   synthesis/place_and_route tools from the vendors mostly handle this?
3. Are there any issues that completely go away when you move from software to hardware? E.g. when
   moving from CPU to (Nvidia) GPU programming, you can directly control the cache and the issue of
   cache thrashing goes away (replaced by other issues of course).
4. In CUDA the design pressure is throughput, is this ever the case on the FPGA in HFT, or is
   latency always the boss?
5. How is the boundary drawn between what stays in RTL and what is implemented in software?
6. At a high level, what are the dominant considerations when designing RTL for HFT (like in CUDA,
   you consider register pressure, dram stalls, occupancy).

1. Does an FPGA hot path ever need to access memory from DRAM? Does the low-level control offered by
   an FPGA allow you to escape the jitter that is expected when a general-purpose machine needs to
   access DRAM and may clash with the DRAM refresh?
2. Do you guys need to worry about parasitic effects on the FPGA, or do the
   synthesis/place_and_route tools from the vendors mostly handle this?
3. Are there any issues that completely go away when you move from software to hardware? E.g. when
   moving from CPU to (Nvidia) GPU programming, you can directly control the cache and the issue of
   cache thrashing goes away (replaced by other issues of course).
4. In CUDA the design pressure is throughput — keep thousands of threads occupied and hide latency.
   On these FPGAs it sounds like the opposite: a single deeply-pipelined path where you'd rather have
   one packet through with minimal jitter than maximal throughput. How does that change how you think
   about a design, and is there ever still a place for wide parallelism on the card?
5. What does "fast" iteration look like when a place-and-route can take hours? Do you lean on
   simulation/co-simulation the way a CUDA dev leans on a quick recompile, and how much of the design
   gets validated before anything hits real silicon?
6. Where's the boundary drawn between what stays in RTL and what gets pushed to the host CPU? In CUDA
   that line (host vs device) is fixed by the API; here it sounds negotiable — what makes you decide
   a piece of logic earns its place in the fabric vs. living in software?
7. How fixed is timing closure as a constraint? Is hitting the target clock the thing that quietly
   dominates the schedule, the way register pressure / occupancy quietly dominates a CUDA kernel?

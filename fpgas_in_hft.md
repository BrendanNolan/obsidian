# Why FPGA driver work at an HFT shop is interesting (if you know CUDA)

For someone with CUDA experience, FPGA driver work at an HFT shop hits a lot of the same nerves —
but the constraints are sharper:

**The mental model transfers, but the units shrink.** CUDA trains you to think about data movement,
memory hierarchies, and where the parallelism actually lives. On FPGAs in HFT the same instincts
apply, but the budget collapses from "milliseconds for a kernel launch" to "nanoseconds, and the
PCIe round-trip is now your enemy." You'll obsess over the same things — DMA, pinned memory (memory
that must stay at the same physical address and can't be moved to a new page behind the same virtual
address), cache lines, NUMA pinning — except now they dominate everything instead of being an
optimization pass.

**You stop hiding behind an abstraction.** CUDA gives you a runtime, a driver, a scheduler, a memcpy
that mostly does the right thing. Writing FPGA drivers means _you are_ that layer. You decide how
userspace talks to the card, whether you go through the kernel at all (often you don't — DPDK-style
kernel bypass, hugepages, busy-poll), how interrupts vs. polling are traded off, how you signal
completion without a syscall. It's the part of the GPU stack NVIDIA writes for you, and now you
write it.

**The hardware is malleable in a way GPUs aren't.** A GPU is a fixed architecture you feed work to.
An FPGA's pipeline is co-designed with the driver — the hardware team can move a queue, change a
register layout, or push logic across the PCIe boundary if it shaves latency. Driver work becomes a
conversation with the RTL, not just a consumer of someone else's ISA. If you liked the "what is this
machine actually doing" part of CUDA, that part gets much louder.

**The feedback loop is brutal and addictive.** In HFT, a 50ns improvement is a real, measurable edge
— and you can see it on a scope or in tick-to-trade histograms the same day. Compared to CUDA work
where wins are often "30% faster training," the signal is much crisper: nanoseconds, P&L, done.

**The adjacent surface is unusually rich.** You end up touching kernel internals, NIC offload,
PTP/clock sync, lock-free ring buffers, the C++ that sits on top, and the market-data protocols
feeding the card. CUDA people tend to like systems-y problems; this is a denser concentration of
them than most jobs offer.

The honest tradeoff: tooling is worse than CUDA's, the community is tiny and secretive (HFT firms
don't publish), and "interesting hardware problem" sometimes means "three weeks chasing a PCIe
ordering bug." But if what drew you to CUDA was the closeness to the metal, FPGA drivers in HFT is
essentially that turned up to 11.

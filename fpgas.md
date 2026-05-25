# PCIe and the round-trip

**PCIe** (PCI Express) is the high-speed serial bus that connects expansion cards — GPUs, NVMe SSDs,
NICs, FPGAs — to the CPU and main memory. It's the physical "pipe" data travels over to get between
the host and a device sitting in a slot on the motherboard. Modern HFT cards typically sit on PCIe
Gen4 or Gen5 x8/x16 lanes.

**The round-trip** is the time it takes for a transaction to go from the CPU out across PCIe to the
device, and for the response (or completion) to come back. Two flavors matter:

- **MMIO read round-trip** — the CPU reads a register on the card. The read can't be cached or
  speculated past, so the core stalls until the device answers. On a modern system that's roughly
  **300–800 ns**, often more. Compare to an L3 hit (~10 ns) or a DRAM access (~80 ns) and you see
  why it's painful.
- **DMA round-trip** — the card writes a packet/result into host memory and signals completion
  (interrupt or a memory flag the CPU polls). Latency depends on the path, but the device-initiated
  direction is much cheaper than a CPU-initiated read.

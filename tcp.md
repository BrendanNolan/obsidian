# TCP

## Definitions

- `Port`: An id in software to allow a single host to distinguish TCP data streams
- `Socket`: The concatenation of a host's network identifier (usually an IP address) and one of the
  host's ports
- `Connection`: From the TCP RFC: "The reliability and flow control mechanisms described above
  require that TCPs initialize and maintain certain status information for each data stream. The
  combination of this information, including sockets, sequence numbers, and window sizes, is called
  a connection. **Each connection is uniquely specified by a pair of sockets identifying its two
  sides.**"

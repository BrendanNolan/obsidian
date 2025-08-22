# TCP

## Definitions

**_Definition_** A `port` is id in software to allow a single host to distinguish TCP data streams.

**_Definition_** A `Socket` is the concatenation of a host's network identifier (usually an IP
address) and one of the host's ports.

**_Definition_** From the TCP RFC: "The reliability and flow control mechanisms described above
require that TCPs initialize and maintain certain status information for each data stream. The
combination of this information, including sockets, sequence numbers, and window sizes, is called a
`connection`. Each connection is uniquely specified by a pair of sockets identifying its two sides."

**_Definition_** Each byte of data has - conceptually - a `sequence number`. The sequence number of
the first byte of data in a segment is transmitted with that segment and is called the
`segment sequence number`. Segments also carry an `acknowledgment number` which is the sequence
number of the next expected data byte of transmissions in the reverse direction.

**_Definition_** To govern the flow of data between TCPs, a flow control mechanism is employed. The
receiving TCP reports a `window` to the sending TCP. This window specifies the number of bytes,
starting with the acknowledgment number, that the receiving TCP is currently prepared to receive.

## File-Like Interface

The TCP/user interface provides for calls made by the user on the TCP to `OPEN` or `CLOSE` a
connection, to `SEND` or `RECEIVE` data, or to obtain `STATUS` about a connection. These calls are
like other calls from user programs on the operating system, for example, the calls to open, read
from, and close a file.

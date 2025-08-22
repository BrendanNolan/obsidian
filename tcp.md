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

A connection is specified in the `OPEN` call by the local port and foreign socket arguments. In
return, the TCP supplies a (short) local connection name by which the user refers to the connection
in subsequent calls. There are several things that must be remembered about a connection. To store
this information we imagine that there is a data structure called a Transmission Control Block
(TCB). One implementation strategy would have the local connection name be a pointer to the TCB for
this connection. The `OPEN` call also specifies whether the connection establishment is to be
actively pursued, or to be passively waited for.

A passive `OPEN` request means that the process wants to accept incoming connection requests rather
than attempting to initiate a connection. Often the process requesting a passive `OPEN` will accept
a connection request from any caller. In this case a foreign socket of all zeros is used to denote
an unspecified socket. Unspecified foreign sockets are allowed only on passive `OPEN`s.

Processes can issue passive OPENs and wait for matching active OPENs from other processes and be
informed by the TCP when connections have been established. Two processes which issue active OPENs
to each other at the same time will be correctly connected. This flexibility is critical for the
support of distributed computing in which components act asynchronously with respect to each other.

Processes can issue passive OPENs and wait for matching active OPENs from other processes and be
informed by the TCP when connections have been established. Two processes which issue active OPENs
to each other at the same time will be correctly connected. This flexibility is critical for the
support of distributed computing in which components act asynchronously with respect to each other.

# TCP

## Overview

### Definitions

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

### File-Like Interface

The TCP/user interface provides for calls made by the user on the TCP to `OPEN` or `CLOSE` a
connection, to `SEND` or `RECEIVE` data, or to obtain `STATUS` about a connection. These calls are
like other calls from user programs on the operating system, for example, the calls to open, read
from, and close a file.

A connection is specified in the OPEN call by the local port and foreign socket arguments. In
return, the TCP supplies a (short) local connection name by which the user refers to the connection
in subsequent calls. There are several things that must be remembered about a connection. To store
this information we imagine that there is a data structure called a Transmission Control Block
(TCB). One implementation strategy would have the local connection name be a pointer to the TCB for
this connection. The OPEN call also specifies whether the connection establishment is to be actively
pursued, or to be passively waited for.

A passive OPEN request means that the process wants to accept incoming connection requests rather
than attempting to initiate a connection. Often the process requesting a passive OPEN will accept a
connection request from any caller. In this case a foreign socket of all zeros is used to denote an
unspecified socket. Unspecified foreign sockets are allowed only on passive OPENs.

Processes can issue passive OPENs and wait for matching active OPENs from other processes and be
informed by the TCP when connections have been established. Two processes which issue active OPENs
to each other at the same time will be correctly connected. This flexibility is critical for the
support of distributed computing in which components act asynchronously with respect to each other.

Processes can issue passive OPENs and wait for matching active OPENs from other processes and be
informed by the TCP when connections have been established. Two processes which issue active OPENs
to each other at the same time will be correctly connected. This flexibility is critical for the
support of distributed computing in which components act asynchronously with respect to each other.

If there are several pending passive OPENs (recorded in TCBs) with the same local socket, an foreign
active OPEN will be matched to a TCB with the specific foreign socket in the foreign active OPEN, if
such a TCB exists, before selecting a TCB with an unspecified foreign socket.

### Control Flags

The procedures to establish connections utilize the synchronize (SYN) control flag and involves an
exchange of three messages. This exchange has been termed the `three-way hand shake`.

A connection is initiated by the rendezvous of an arriving segment containing a SYN and a waiting
TCB entry each created by a user OPEN command. The matching of local and foreign sockets determines
when a connection has been initiated. The connection becomes "established" when sequence numbers
have been synchronized in both directions.

The clearing of a connection also involves the exchange of segments, in this case carrying the FIN
control flag.

The data that flows on a connection may be thought of as a stream of bytes. The sending user
indicates in each SEND call whether the data in that call (and any preceeding calls) should be
immediately pushed through to the receiving application by the setting of the `PUSH` flag (e.g.
keystrokes are being sent over TCP, the sender may set the PUSH flag so that the receiving user will
see the keystrokes appearing on their screen immediately). **Note** The PUSH flag is not about
sending data over the TCP connection, it is about delivering data from the TCP endpoint (which may
be buffering incoming data and delivering it up to the application only when the buffer fills up) up
to the receiving application.

### Robustness Principle

**The Robustness Principle:** TCP implementations will follow a general principle of robustness: be
conservative in what you do, be liberal in what you accept from others.

## Functional Specification

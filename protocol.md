# UKHAS.net Protocol

## Layer 1 (Radio)

UKHAS.net data is transmitted within the 863 - 870 MHz license-exempt
frequency band, on a centre frequency of 869.500MHz.

Data is modulated using 2000 baud 2FSK encoding, with a total shift of
24kHz (12kHz above the centre frequency for '1', 12kHz below the centre
frequency for '0').

Bytes are transmitted most significant bit first.

## Layer 2 (Framing)

The low-level framing format consists of:

1. **Preamble** of at least three bytes of `0xAA`.
2. Two **synchronisation bytes**: `0x2D 0xAA`.
3. A single **length byte** between 0 and 64 (`0x00` and `0x40`).
4. The number of **data bytes** as described by the length byte, in the
   format described in the Layer 3 section below.
5. Two **checksum bytes**, calculated as described below.

No Manchester coding or Data Whitening are used, so the application must ensure
sufficient bit transitions in the data to retain synchronisation

### Checksum
Checksum is CRC16 with a polynominal `0x1021` and starting bits `0x1D0F`,
XORd with `0xFFFF` at the end, calculated over the length and data fields.

### Example

`0xAA, 0xAA, 0xAA, 0x2D, 0xAA, Length, Data, Data, Data, …, CRC16, CRC16`

## Layer 3 (packet)

The data bytes of the packet contain only ASCII characters. Packets are
structured as follows:

1. **Time-To-Live counter** (TTL). A single-byte ASCII integer which
   describes how many times the packet can be repeated.
    * A repeater node should decrement this number before repeating the
      packet.
    * If a repeater node receives a packet with a TTL of 0, it must not
      be repeated.
    * Nodes broadcasting their own packets are recommended to initially
      set this counter to 3.
2. **Sequence Counter**. A single lowercase character between `a` and
   `z`.
    * On startup, a node should transmit a packet with a sequence counter
      of `a`.
    * Subsequent packets should transmit sequential sequence counters
      (`b`, `c`, `d`, ...), wrapping round from `z` to `b`. The sequence
      counter `a` should only be transmitted on node startup.
3. A number of **Fields**, which, which the exception of the special
   comment field, consist of a single uppercase letter, followed by a
   comma-separated list of values. For more information see the [list of
   fields](fields.md). The data section is variable in length.
4. The **Path**, enclosed within `[` and `]`:
    * When broadcasting a new packet, the node should include its node
      ID in the Path (e.g. `[NODEID]`).
    * When repeating a packet, the repeater node should append a comma,
      followed by its node ID to the Path (e.g.
      `[NODEID,REPEATERID]`).

The canonical specification of the packet format can be found in the
[formal grammar](grammar.ebnf).

### Example Packet
`2iL51.498,-0.0527T21R0[AB,AA]`

### Node IDs

Node IDs consist of up to 16 upppercase letters.

When assigning a new Node ID, check that the ID is not already taken on
the [node list](https://ukhas.net/nodeList).

### Repeating

A node configured as a repeater should rebroadcast packets it receives.
When doing so it must follow these rules:

1. If the Time-to-Live counter is 0, do not repeat.
2. If the repeater's node ID already exists in the Path, do not repeat.
3. If `<packet length> + <repeater node ID length> + 1 > 64`, do not
   repeat.
4. Append a comma and the repeater's node ID to the Path.
5. Wait for a random interval between 0 and 1000ms, specific to the
   repeater node, to avoid collisions.
6. Broadcast the packet.

### Gatewaying

A node configured as a gateway should submit all packets it receives
to the UKHAS.net server using the [packet submission
protocol](https://ukhas.net/wiki/uploading_packets).

# UKHAS.net Protocol

## Layer 1

* 869.5MHz centre frequency
* 24kHz total shift, deviating 12kHz above centre for '1'
* 2000 baud
* 2FSK
* all bytes transmitted most significant bit first

## Layer 2

* 3 byte preamble, 2 byte sync sequence, Length, Data Bytes, CCITT
  CRC-16 checksum
* Preamble: 0xAA, 0xAA, 0xAA
* Synchronisation word: 0x2D then 0xAA, transmitted MSb first
* No address byte
* Single byte length (0  - 255) - currently limited to 1 - 64 by application layer.
* No Manchester coding or Data Whitening - so the application must ensure
  sufficient bit transitions in the data to retain synchronisation
* Checksum is polynominal 0x1021 and starting bits 0x1D0F, XORd with
  0xFFFF at the end, calculated over the length and data fields,
  transmitted MSB and MSb first.

Example: `0xAA, 0xAA, 0xAA, 0x2D, 0xAA, Length, Data, Data, Data, ….,
CRC16, CRC16`
Frame contains “Length” bytes of “Data”

## Guide to Packet Format

`2iL51.498,-0.0527T21R0[AB,AA]`

1. Repeat Number: The first byte contains the number of repeats - most
   commonly this is set to 3 however if you want the packet to go further
   then you can increase this number. Everytime a packet is repeated the
   node subtracts 1 from this number, once it reaches 0 the packet will not
   be repeated.
2. Sequence Count: this cycles from 'b' to 'z' allowing packets to be
   distinguished, 'a' is only transmitted on boot of the node.
3. Data: this is where the main packet data is placed, for example
   location or temperature. Each data type starts with an identifying
   character and then each value separated by a comma. E.g.  `T18.5,23,10`
   For more information see the [list of fields](fields.md). The data section
   is variable in length but ideally the shorter the packet the better.
4. Path: enclosed within '[' and ']' each node adds its node ID
   separated by a comma at the end. Each node ID can be up to a maximum of
   16 bytes. Nodes only repeat packets that they haven't repeated before
   (to avoid loops) so before repeating the node needs to check for its
   node ID in the path section. For example (pseudocode):
```
1) detect final ] in string/array
2) replace ] with ,your_ID] (comma followed by your_ID followed by
   square end bracket)
````

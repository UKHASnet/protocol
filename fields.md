# Packet Data Types

This is a user-friendly summary of the data types used in a UKHAS.net
packet. The canonical list can be found in the [formal
grammar](grammar.ebnf).

## List of data types

| Type          | Unit           | Letter | Example       | Comment |
|---------------|----------------|--------|---------------|---------|
| Voltage       | Volts          | V      | V4.1          |  |
| Current       | Amperes        | I      | I0.1          |  |
| Temperature   | Celcius        | T      | T-8.2         | |
| Relative Humidity | Percent    | H      | H40           | |
| Atmospheric Pressure | Pascals | P      | P101412       | |
| Light         | (undefined)    | S      | S12           | |
| Wind          | m/s, degrees   | W      | W15,355       | One or two elements (speed, bearing) |
| RSSI          | dBmW           | R      | R-88,-96      | One or two elements (RSSI of last received packet, receiver noise floor) |
| Zombie        | Boolean        | Z      | Z1            | Whether node is a "Zombie" (not acting as a repeater) |
| Location      | Degrees, meters | L     | L51.5,-1.3901 | Two or three elements (lat, long or lat, long, altitude) |
| Packet Count  |                | C      | C16           | Number of packets received by node |
| Custom        |                | X      | X3,1,23       | Custom data |
| Comment       |                | :      | :This is a comment | All characters permitted except '[', ']' and '\|'. Must be the last field. |

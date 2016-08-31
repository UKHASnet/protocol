# Packet Data Types.

## Lx.x,x.x
* Location in Signed Decimal Degrees
* Given as floating-point, eg L51.5,-1.3901
* Compulsory: At least 2 elements (latitude, longitude)
* Optional: Upto 3 elements (latitude,longitude,altitude)

## Vx.x
* Voltage in Signed Volts
* Given as floating-point, eg V3.73
* Compulsory: At least 1 element
* Optional: Infinite comma seperated elements

## Tx.x
* Temperature in Signed Degrees Celsius
* Given as floating-point, eg T-8.2
* Compulsory: At least 1 element
* Optional: Infinite comma seperated elements

## Hx
* Relative Humidity in percentage
* Given as integer, eg H40
* Compulsory: At least 1 element
* Optional: Infinite comma seperated elements
* Note: This is currently handled as floating point in the ukhas.net database

## Px
* Atmospheric Pressure in Pascals
* Given as integer, eg P101412
* Compulsory: At least 1 element
* Optional: Infinite comma seperated elements
* Note: This is currently handled as floating point in the ukhas.net database

## Wx,x
* Wind Speed in m/s, (Optional Bearing in Degrees)
* Given as integer, eg W15,355
* Compulsory: 1 element (Wind Speed)
* Optional: Up to 2 elements (Wind Speed, Wind Bearing)

## Rx
* Last RSSI in dBmW
* Given as integer, eg R-88
* Compulsory: At least 1 element
* Optional: Infinite comma seperated elements
* Note: This is currently handled as floating point in the ukhas.net database

## Zx
* Zombie-mode status
* Given as integer: '0' means non-zombie (repeater), '1' means zombie
* Compulsory: Only 1 element

## :xxx
* Custom message or comment
* Given as lowercase string, eg :hello
* All characters permitted except '[', ']' and '|'
* Note: Not fully supported by the parser, but work is being done on this and irc bot integration (March 2016)

## Cx
* Count - number of packets rx'd by the node (ensure is unsigned)
* Compulsory: At least 1 element

## Sx
* Sun - value from photoresistor
* Compulsory: At least 1 element
* Treated as floating-point by the ukhas.net database

## Xx
* Custom data, not graphed on ukhas.net
* Compulsory: At least 1 element

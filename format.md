# PES (Brother) Design format

## Explanation of Abbreviations

* HEX 
= hexadecimal
* DEC
= decimal
* MSB
= Most significant Bit
* LSB
= Least significant Bit
* NN
= Value varies or unknown
 
## Notes
* uses PEC code block stored in PES file for decoding
* also works with PES type 2 files
* depending on type of stitch, each stitch has two to four bytes !

## File structure

Address |	Variable name |	Explanation 
------- |   ------------- | -----------    
HEX 0008 - 0010	| pecstart	| 3 Bytes, pointing to beginning of PEC codeblock stored in PES file, LSB first
pecstart + 49   |           | No. of colors in file
pecstart + 515	| graphic	| 3 Bytes pointing to beginning of pixel graphic, LSB first
pecstart + 521	| 	        | 2 Bytes, x-size of design, LSB first
pecstart + 523	|           | 2 Bytes, y-size of design, LSB first
pecstart + 533	|           | Beginning of stitch data
pecstart + graphic + 512 | 	| End of stitch data
pecstart + graphic + 513 |  | 228 Byte pixel graphic, each bit = one pixel, drawn from top left to bottom right

## Examples

Byte values |	No. of Bytes | 	Explanation
----------- |   ------------ |  -----------
kx=254, ky=176, NN	 | 3	| Color change, Byte following ky gives color No. Stitch data (2-4 Bytes) follows
128 <= kx <= 254, ky | 	2	| Jump stitch, lower four bytes (nibble) of kx is multiplication factor for jump stitch ky, direction of jump is determined as follows:
  | | | (kx and 15) <= 7		jump in positive direction. length = ky + (kx and 15) x 256
  | | | (kx and 15) >=8		jump in negative direction. length = (ky-256) + ((kx and 15)-15) x 256
kx, ky | 	2	| Regular stitch data
  | | | 0 <= kx <= 63 : positive
  | | | 64 <= kx <= 127 : negative, kx=kx-128
  | | | 0 <= ky <= 63 : positive
  | | | 64 <= ky<= 127 : negative, ky=ky-128
kx = 255, ky = 0	| 2	| End of stitch data



## links
* http://www.achatina.de/sewing/main/TECHNICL.HTM
* https://github.com/torvalds/pesconvert    converts PES to picture
* https://github.com/njcrawford/EmbroideryReader  PES viewer
* https://github.com/Embroidermodder/Embroidermodder  open source embrodiery design





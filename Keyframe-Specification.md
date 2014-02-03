# General Structure
 - Header (0x10 bytes)
 - n Player Segments (one for each player)
   - Header (0x17)
   - Summoner name (0x10)
   - Unknown data (0x6C)
   - Champion name (0x??)
   - Unknown data (0x??)
   - Rune Page (0x78)
     - Marks (0x24)
     - Seals (0x24)
     - Glyphs (0x24)
     - Quints (0x0C)
   - Mastery Page (0x??)
     - Header (0x04)
     - n Entries (one for each mastery, points in the same mastery do not count as a new entry)
     - Padding
   - Entity State Data (?)
     - Header (0x09)
     - 9 item entries (0x3F)
     - Unknown data
   - Footer (0x05)
 - Turret Segments
   - Unknown data
 - Unknown data

Note: everything is in little endian.
# KeyFrame Header
```
03 XX XX XX             
XX 02 00 00
00 61 00 00
00 00 00 00
```
# Player Segment
Note: all offsets are relative to the first byte of the player segment
## Header
offset: 0x00; length: 0x17
```
B3
XX C3 4B 00                 // first byte is usually 00 but is sometimes 01. death?
XX 00 00 40                 // entity ID, increments for each player in order of the file (19 00 00 40, 1A 00 00 40, etc.)
XX 00 00 00                 // each player is given a number from 0 to 9, not sure of ordering
40 00 XX 00
XX FF XX 00
00 00
```
## Summoner Name
bytes after the null are meaningless
offset: 0x17; length: 0x10
Example:
```
4D 61 61 61  61 61 61 61  61 61 61 61  61 72 72 72  // "Maaaaaaaaaaaarrr" (max length)
```
Example:
```
51 75 69 63  6B 00 F7 11  45 00 00 00  00 00 00 00  // "Quick"
```
Example:
```
51 75 69 63  6B 00 BE B1  43 00 00 00  00 C4 DA 3C  // "Quick"
```
Example:
```
41 6D 65 63  00 76 E9 07  42 00 00 00  00 00 00 00  // "Amec"
```
Example:
```
57 61 72 4C  6F 72 44 20  54 69 74 61  6E 00 DC 34  // "WarLorD Titan"
```
## Unknown data
offset: 0x27; length: 0x6C
```
XX XX XX XX  XX XX XX XX  XX 00 00 00  00 00 00 XX
XX XX XX XX  XX XX XX XX  XX XX XX XX  XX XX XX XX
XX XX XX XX  XX XX XX XX  XX XX XX XX  XX 02 00 00
00 XX XX XX  XX 01 00 00  00 XX XX XX  XX XX XX XX
00 23 E0 XX  XX XX XX XX  XX XX XX XX  XX 0D 00 00
00 XX XX XX  XX XX XX XX  00 DD 14 XX  XX 00 00 XX
XX 00 00 00  00 XX XX XX  XX XX XX XX  00 C2 DC XX
```
## Champion Name
offset: 0x97; length: 0x10
```
48 65 69 6D  65 72 64 69  6E 67 65 72  00 88 DB 18   // I think any data after the first 00 is meaningless
```
## Unknown data
offset: 0xA7; length: 0x2C
```
00 A6 94 XX
!! XX XX XX  XX XX XX XX        // the flag marked as !! only ever equals 0 or 1 
XX XX XX XX  XX XX XX XX
00 60 96 XX  XX XX XX XX
XX XX XX XX  !! 83 !! 12        // first flag: 0 or 2; second: 0 or 1
02 00 00 29  EN TI TY ID
```
## Rune Page
offset: 0xE3; length: 0x78
### Marks
Example:
```
83 14 00 00
85 14 00 00
85 14 00 00
85 14 00 00
85 14 00 00
85 14 00 00
85 14 00 00
85 14 00 00
85 14 00 00
```
### Seals
Example:
```
A9 14 00 00
A9 14 00 00
A9 14 00 00
A9 14 00 00
A9 14 00 00
A9 14 00 00
A9 14 00 00
A9 14 00 00
A9 14 00 00
```
### Glyphs
Example:
```
C9 14 00 00
C9 14 00 00
C9 14 00 00
C9 14 00 00
C9 14 00 00
C9 14 00 00
C9 14 00 00
C9 14 00 00
C9 14 00 00
```
### Quints
Example:
```
D7 14 00 00
24 15 00 00
24 15 00 00
```
## Mastery Page
offset: 0x14B; length: 0x194
### Header
``` 
24 4F 36 06             // only present in some packets (was not in source gist)?
A8 6E 49 06             // header?
```
### Mastery Entry

```
XX                      // the mastery id; does not correlate with AIR Client talentId
XX                      // the tree; 74 = offensive, 75 = defensive, 76 = utility (note: double check)
03 00                   // unknown
XX                      // number of points in mastery (max 4)
```
### Padding
```
00 00 00 00 ...
```
## Entity State
Note: not even sure this is what this actually is
### Header
Second byte changes.
offset: 0x2F2, length: 0x09
```
1E XX B3 00 65 FE 00 0C 01
```
### Item Entries
There are 9 item entries (future proofing?)
The first 7:
```
XX XX 00 00             // Item ID
XX                      // Index (slot); 00 to 08
XX                      // Quantity; 01 to 05
XX                      // Charges
```
8th:
```
D1 07 00 00             // This item ID does not map to any item
07
01
00
```
9th:
```
00 00 00 00
08
00
00
```
### Unknown data
Example:
```
00 00 80 BF             // 3,212,836,864  OR  0 - 49,024  OR  0 - 0 - 128 - 191
00 0E 8E 40             // why is this different? It's the same sometimes.
00 00 80 BF
00 00 80 BF
00 00 80 BF
00 00 80 BF
00 00 80 BF
00 00 80 BF
00 00 80 BF
```
### Unknown data
0x130 bytes long
Example:
```
A3 00 28 01  00 00 45 00  28 01 00 00  06 00 00 00
00 00 00 00  07 00 00 00  0F 00 00 00  01 00 00 00
00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00
F8 26 1C 46  00 6C 02 46  00 00 00 00  00 00 00 00
00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00
00 00 00 00  00 00 00 00  00 00 00 00  1F 00 00 00
03 00 00 00  93 85 30 43  03 00 00 00  02 00 00 00
00 00 00 00  CB 0A 26 44  08 A0 6C 45  C4 CC 7F 43
CC 13 E5 45  17 00 00 00  00 00 4B 00  00 00 05 00
00 00 46 00  00 00 04 00  00 00 00 00  00 00 8B AA
AF 47 48 23  42 46 CF 45  0A 46 00 00  00 00 00 00
00 00 00 00  00 00 00 00  00 00 3E 00  00 00 00 00
00 00 44 00  00 00 09 00  00 00 04 00  00 00 13 00
00 00 00 00  00 00 64 00  00 00 00 00  00 00 00 00
00 00 00 00  00 00 7B 7F  D4 47 CB 6F  5D 46 EA 77
86 46 B1 09  00 00 CD 04  BE 43 18 4B  00 43 01 00
00 00 00 00  00 00 12 80  6B 46 AD 6A  BA 44 11 01
81 44 00 00  00 00 00 00  00 00 00 00  00 00 00 00
00 00 07 00  00 00 00 00  00 00 00 00  00 00 00 00
```
### Some kind of marker
```
B3 00 03 15
```
### Ability data
XX = points in ability (0 to 5)
```
00 00 XX 00 F3 00 YY
00 01 XX 00 F3 00 YY
00 02 XX 00 F3 00 YY
00 03 XX 00 B3 00 ZZ
```
### String data
Variable length
Not sure why these specific strings are here
Example: 
```
17 00 2C 01
6C 75 63 69 61 6E 72 6D 69 73 73 69 6C 65 00 // "lucianrmissile"
F3 00 1E 00 32 01
63 72 65 73 74 6F 66 74 68 65 61 6E 63 69 65 6E 74 67 6F 6C 65 6D 6C 69 6E 65 73 00 // "crestoftheancientgolemlines"
F3 00 10 00 33 01
6C 75 63 69 61 6E 71 64 61 6D 61 67 65 00
```
Example:
```
17 00 01 01
76 69 77 00 // "viw"
F3 00 0A 00 2F 01
76 69 72 64 75 6E 6B 00 F3 00 // "virdunk"
17 00 32 01
69 74 65 6D 70 6C 61 63 65 6D 65 6E 74 6D 69 73 73 69 6C 65 00 // "itemplacementmissile"
```
### Unknown data
Variable length
Example:
```
B3  00 09 84 00  40 00 FE 4C  40 00 00 80
BF  F3 00 09 00  43 C0 DD C5  41 00 00 80
BF  F3 00 09 00  05 00 DC 9D  40 00 00 80
BF  F3 00 09 00  0A 00 DA 1E  40 00 00 80
BF  F3 00 09 00  40 D0 CE B8  42 00 00 80
BF  F3 00 09 00  41 80 BB 1F  42 00 00 80
BF  B3 00 01 EF  00 01 93 00  07 FE 00 00
```
Example:
```
B3  00 09 84 00  40 E0 85 2E  43 00 00 80
BF  F3 00 09 00  41 58 B1 85  43 00 00 80
BF  B3 00 01 EF  00 01 93 00  07 FE 00 00
```
## Footer
```
00 00 15 01  EN TI TY ID
```
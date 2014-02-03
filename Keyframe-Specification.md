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
```
4D 61 61 61  61 61 61 61  61 61 61 61  61 72 72 72  // "Maaaaaaaaaaaarrr" (max length)
```
```
51 75 69 63  6B 00 F7 11  45 00 00 00  00 00 00 00  // "Quick"
```
```
51 75 69 63  6B 00 BE B1  43 00 00 00  00 C4 DA 3C  // "Quick"
```
```
41 6D 65 63  00 76 E9 07  42 00 00 00  00 00 00 00  // "Amec"
```
```
57 61 72 4C  6F 72 44 20  54 69 74 61  6E 00 DC 34  // "WarLorD Titan"
```
## Unknown data
offset: 0x27; length: 0x6C
```
00 00 00 00  00 02 00 00  00 00 00 00  00 00 00 75
02 D0 C8 FA  09 00 00 00  00 00 00 00  00 40 00 E4
03 E6 27 DD  45 00 00 00  00 00 00 00  00 02 00 00
00 68 C0 FA  09 01 00 00  00 C8 C8 FA  09 0C DB 3C
02 23 E0 A5  77 90 4A 76  02 D0 C8 FA  09 0D 00 00
00 C8 C8 FA  09 20 DB 3C  02 DD 14 53  77 00 00 75
02 00 00 00  00 D0 C8 FA  09 34 DB 3C  02 C2 DC DD
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
XX XX XX XX  XX XX XX XX
XX XX XX XX  XX XX XX XX
00 60 96 XX  XX XX XX XX
XX XX XX XX  XX 83 XX 12
02 00 00 29  EN TI TY ID
```
## Rune Page
offset: 0xE3; length: 0x78
### Marks
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
```
//no idea what this is
00 00 80 BF             // 3,212,836,864  OR  0 - 49,024  OR  0 - 0 - 128 - 191
00 0E 8E 40             // why is this different? It's the same sometimes.
00 00 80 BF
00 00 80 BF
00 00 80 BF
00 00 80 BF
00 00 80 BF
00 00 80 BF
00 00 80 BF

//unknown (comparing multiple keyframes)
A3 00 28 01  00 00 45 00  28 01 00 00  0A 00 00 00
A3 00 28 01  00 00 45 00  28 01 00 00  00 00 00 00

00 00 00 00  09 00 00 00  10 00 00 00  02 00 00 00
00 00 00 00  00 00 00 00  05 00 00 00  00 00 00 00

00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00
00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00

9F 8B 46 46  00 B8 1A 46  00 00 00 00  00 00 00 00
3C C0 6D 44  00 00 5C 44  00 00 00 00  00 00 00 00

00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00
00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00

00 00 00 00  00 00 00 00  00 00 00 00  22 00 00 00
00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00

04 00 00 00  9B 47 00 44  03 00 00 00  02 00 00 00
00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00

00 00 00 00  CB 0A 26 44  C4 6B 00 46  EF 2A A6 44
00 00 00 00  C4 98 55 43  3D BF 91 41  3D BF 91 41

F8 CF 40 46  25 00 00 00  00 00 54 00  00 00 06 00
1C A0 26 44  0A 00 00 00  00 00 00 00  00 00 00 00

00 00 4E 00  00 00 05 00  00 00 00 00  00 00 35 99
00 00 00 00  00 00 01 00  00 00 00 00  00 00 E1 6B

E2 47 A2 60  88 46 6F 0F  45 46 00 00  00 00 00 00
C5 44 41 53  E5 43 AC E4  83 44 00 00  00 00 00 00

00 00 00 00  00 00 00 00  00 00 54 00  00 00 00 00
00 00 00 00  00 00 00 00  00 00 05 00  00 00 02 00

00 00 5A 00  00 00 0C 00  00 00 05 00  00 00 15 00
00 00 03 00  00 00 00 00  00 00 01 00  00 00 01 00  

00 00 00 00  00 00 64 00  00 00 00 00  00 00 00 00
00 00 00 00  00 00 64 00  00 00 00 00  00 00 00 00

00 00 00 00  00 00 6D BB  0C 48 50 5B  A8 46 8E 42
00 00 00 00  00 00 DE F2  CF 44 9A B7  07 44 BC 34

D0 46 53 11  00 00 9C 79  F5 43 50 1D  2D 43 01 00
D7 44 3C 00  00 00 00 00  C0 40 40 6C  48 41 01 00

00 00 00 00  00 00 5F 41  9B 46 FE BF  2C 45 DA 2D
00 00 00 00  00 00 00 00  84 42 00 00  84 42 00 00

D5 44 00 00  00 00 00 00  00 00 00 00  00 00 00 00
00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00

00 00 08 00  00 00 00 00  00 00 00 00  00 00 00 00
00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00

B3 00 03 15  00 00 05 00  F3 00 03 00  01 05 00 F3
B3 00 03 15  00 00 01 00  F3 00 03 00  01 01 00 F3

00 03 00 02  04 00 F3 00  03 00 03 03  00 B3 00 06
00 03 00 02  01 00 F3 00  03 00 03 00  00 B3 00 09

17 00 01 01  76 69 77 00  F3 00 0A 00  2F 01 76 69
84 00 40 46  25 30 43 00  00 80 BF F3  00 09 00 41

72 64 75 6E  6B 00 F3 00  20 00 32 01  62 6C 65 73
B0 79 09 43  00 00 80 BF  B3 00 01 EF  00 01 93 00

73 69 6E 67  6F 66 74 68  65 6C 69 7A  61 72 64 65
07 FE 00 00  00 00 15 01  19 00 00 40  00             //This area seems to have variable size

6C 64 65 72  6C 69 6E 65  73 00 B3 00  09 84 00 43
00 40 14 41  00 00 80 BF  F3 00 09 00  05 00 0E 8E
40 00 00 80  BF F3 00 09  00 40 40 B1  05 43 00 00
80 BF B3 00  01 EF 00 01  93 00 07 FE  00 00 00 00

15 01
```
### Footer
```
EN TI TY ID
00
```
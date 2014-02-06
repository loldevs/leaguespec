# General Structure
 - Header (0x10 bytes)
 - n Player Segments (one for each player)
   - Header (0x17)
   - Summoner name (0x10)
   - Unknown data (0x70)
   - Champion name (0x10)
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
 - n Turret Segments (one for each turret) (0xB4 each)
   - Entity ID (0x04)
   - Constant byte (0x01)
   - Name string (0x1C)
   - Unknown data (0x24)
   - Interesting bytes (0x05)
   - Items (0x46)
     - Item header (0x07)
     - 9 item entries (0x3F)
   - Padding (0x24)
 - Unknown data

Note: everything is in little endian.
# KeyFrame Header
```
03 XX XX XX     // Timestamp, in seconds, as float        
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
offset: 0x27; length: 0x70
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
00 00 80 BF             // -1.000000 (float)
00 0E 8E 40             // why is this different? It's the same sometimes. (4.439209)
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
A3 00 28 01
00 00 45 00 
28 01 00 00 // 296
06 00 00 00 // 6
00 00 00 00 // 0
07 00 00 00 // 7
0F 00 00 00 // 15
01 00 00 00 // 1

00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00

F8 26 1C 46 // 9993.742188
00 6C 02 46 // 8347

00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00
00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00

00 00 00 00 // 0
1F 00 00 00 // 31
03 00 00 00 // 3
93 85 30 43 // 176.521774
03 00 00 00 // 3
02 00 00 00 // 2
00 00 00 00 // 0
CB 0A 26 44 // 664.168640
08 A0 6C 45 // 3786.001953
C4 CC 7F 43 // 255.799866
CC 13 E5 45 // 7330.474609

17 00 00 00 // 23

00 00

4B 00 00 00 // 75
05 00 00 00 // 5
46 00 00 00 // 70
04 00 00 00 // 4
00 00 00 00 // 0
8B AA AF 47 // 89941.085938
48 23 42 46 // 12424.820312
CF 45 0A 46 // 8849.452148

00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00

3E 00 00 00 // 62
00 00 00 00 // 0
44 00 00 00 // 68
09 00 00 00 // 9
04 00 00 00 // 4
13 00 00 00 // 13
00 00 00 00 // 0
64 00 00 00 // 100

00 00 00 00  00 00 00 00  00 00 00 00

7B 7F D4 47 // 108798.960938
CB 6F 5D 46 // 14171.948242
EA 77 86 46 // 17211.957031
B1 09 00 00 // 2481
CD 04 BE 43 // 380.037506
18 4B 00 43 // 128.293335
01 00 00 00 // 1
00 00 00 00 // 0
12 80 6B 46 // 15072.017578
AD 6A BA 44 // 1491.333618
11 01 81 44 // 1032.033325
00 00 00 00 // 0
00 00 00 00 // 0
00 00 00 00 // 0
00 00 00 00 // 0
07 00 00 00 // 7
00 00 00 00 // 0
00 00 00 00 // 0

00 00
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
76 69 72 64 75 6E 6B 00 // "virdunk"
F3 00 17 00 32 01
69 74 65 6D 70 6C 61 63 65 6D 65 6E 74 6D 69 73 73 69 6C 65 00 // "itemplacementmissile"
```
### Unknown data
Variable length

I think these may be floats.

Example:
```
B3 00  09 84 00 40  00 FE 4C 40  00 00 80
BF F3  00 09 00 43  C0 DD C5 41  00 00 80
BF F3  00 09 00 05  00 DC 9D 40  00 00 80
BF F3  00 09 00 0A  00 DA 1E 40  00 00 80
BF F3  00 09 00 40  D0 CE B8 42  00 00 80
BF F3  00 09 00 41  80 BB 1F 42  00 00 80
BF B3  00 01 EF 00  01 93 00 07  FE 00 00
```
Example:
```
B3 00  09 84 00 40  E0 85 2E 43  00 00 80
BF F3  00 09 00 41  58 B1 85 43  00 00 80
BF B3  00 01 EF 00  01 93 00 07  FE 00 00
```
Note: 07 FE 00 00 may signal end of section?
## Footer
```
00 00 15 01  EN TI TY ID  00
```

# Turret Segment
Note: fountain lasers seem to be considered as turrets aswell

## Header
Note: header lenght is not included in offsets
```
93 00 4A 9C      // First turrent
B3 00 4A 9C 01   // All other turrets
```

## Entity ID
offset: 0x00; length: 0x04;
```
XX 00 00 40               //increments for each turret, goes from 01 to 18
```

## Constant byte
offset: 0x04; length: 0x01;
```
40                        //seems to be constantly 40
```

## Name string
offset: 0x05; length: 0x1C;
```
54 75 72 72  65 74 5F 43    // Turret_ChaosTurretShrine_A
68 61 6F 73  54 75 72 72    // This one probably refers to fountain laser
65 74 53 68  72 69 6E 65    
5F 41 00 XX
```

## Unknown data
offset: 0x21; length: 0x24;
```
XX XX 00 00  00 00 XX XX   // second marker seems to always be either 00 00 00 or 60 35 33
XX 00 00 00  5D 02 D5 71    
C3 01 XX XX  34 02 FE FF    
FF FF 8C E3  BE 77 XX XX
04 77 F8 90     
```

## Interesting bytes
offset: 0x45; length: 0x05;
```
!! 00 00 !!  !!             //first marker is either 00 for FLs or 02 for turrets
                            //second marker is either 80 for FLs or 00 for turrets
                            //third marker is either 01 or 02, possibly refers to 
                            //whether or not the turret can be attacked
```

## Items
offset: 0x4A; length: 0x46;
### Header
offset: 0x4A; length: 0x07;
```
B3 00 65 FE  00 0C 01       //the same as players' items' header
```

### Item entries
offset: 0x51; length: 0x3F;
```
DC 05 00 00  00 01 00       // On all towers
DE 05 00 00  01 01 00       // On all towers
DF 05 00 00  02 01 00       // On all towers
DD 05 00 00  03 01 00       // On some towers
00 00 00 00  04 00 00       
00 00 00 00  05 00 00
00 00 00 00  06 00 00 
00 00 00 00  07 00 00
00 00 00 00  08 00 00 
```

## Padding
offset: 0x90; length: 0x24;
```
00 00 80 BF  00 00 80 BF    // Constant (padding?)
00 00 80 BF  00 00 80 BF 
00 00 80 BF  00 00 80 BF 
00 00 80 BF  00 00 80 BF
00 00 80 BF
```



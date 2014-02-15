# Table of content:
  - [Notes](#wiki-notes)
  - [Header](#wiki-header)
  - [Summoner name](#wiki-summoner-name)
  - [Champion name](#wiki-champion-name)
  - [Unknown data](#wiki-unknown-data)
  - [Runes](#wiki-runes)
    - **9** [Marks](#wiki-marks)
    - **9** [Seals](#wiki-seals)
    - **9** [Glyphs](#wiki-glyphs)
    - **3** [Quints](#wiki-quints)
  - [Masteries](#wiki-masteries)
    - [Header](#wiki-header-1)
    - **n** [Mastery entries](#wiki-mastery-entry)
    - [Padding](#wiki-padding)
  - [Items](#wiki-items)
    - [Header](#wiki-header-2)
    - **9** [Item entires](#wiki-item-entry)
    - [Unknown data (padding?)](#wiki-unknown-data-padding)
  - [Unknown data](#wiki-unknown-data-1)
  - [Abilities](#wiki-abilities)
    - [Header](#wiki-header-3)
    - [Ability entities](#wiki-ability-entry)
  - [String data](#wiki-string-data)
  - [Unknown data](#wiki-unknown-data-2)
  - [Footer](#wiki-footer)

# Notes
- Offsets are relative to their parent sections/segments

# Header
- **offset:** ``0x00``
- **length:** ``0x17``

```
B3
!! C3 4B 00                 // Marker #1
++ ++ ++ 40                 // Entity ID
!! 00 00 00                 // Player number
40 00 XX 00
XX FF XX 00
00 00
```

- **Marker #1** can only be ``0x00`` or ``0x01``, could possibly refer to whether the player is dead or not
- **Entity ID** is incremented in the order which players are in the file
- **Entity ID** is usually increased by **1** for each player, however, some IDs may be skipped
- **Player number** is a unique number assigned to each player, ranging from ``0x00`` to ``0x09`` for a 5v5 game
- Ordering of **player number** is unknown

# Summoner Name

- **offset:** ``0x17``
- **length:** ``0x80``

**Example:**

```
53 68 61 65  76 00 20 0F  43 00 00 00  00 00 00 00  // Shaev
00 00 00 00  00 02 00 00  00 00 00 00  00 00 00 4F  
00 78 50 08  08 00 00 00  00 00 00 00  00 64 03 3D  
08 69 43 82  84 00 00 00  00 00 00 00  00 02 00 00  
00 C0 0E 08  08 01 00 00  00 70 50 08  08 CC D0 2A
00 23 E0 1D  77 98 45 50  00 78 50 08  08 0D 00 00
00 70 50 08  08 E0 D0 2A  00 DD 14 EA  74 00 00 4F
00 00 00 00  00 78 50 08  08 F4 D0 2A  00 C2 DC B7
```

- bytes after first ``0x00`` byte are meaningless

# Champion Name

- **offset:** ``0x97``
- **length:** ``0x10``

```
4C 65 62 6C  61 6E 63 00  00 78 50 08  08 08 D1 2A  // Leblanc
```

- bytes after first ``0x00`` byte are meaningless

# Unknown data

- **offset:** ``0xA7``
- **length:** ``0x2C``

```
00 A6 94 XX
!! XX XX XX  XX XX XX XX        // Marker #1
XX XX XX XX  XX XX XX XX
00 60 96 XX  XX XX XX XX
XX XX XX XX  !! 83 !! 12        // Marker #2; Marker #3
02 00 00 29  ++ ++ ++ 40        // Entity ID
```

- **Marker #1** can only be ``0x00`` or ``0x01``
- **Marker #2** can only be ``0x00`` or ``0x02``
- **Marker #3** can only be ``0x00`` or ``0x01``

# Rune Page

- **offset:** ``0xD3``
- **length:** ``0x78``

## Marks

**Example:**

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

## Seals

**Example:**

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

## Glyphs

**Example:**

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

## Quints

**Example:**

```
D7 14 00 00
24 15 00 00
24 15 00 00
```

# Mastery Page

- **offset:** ``0x14B``
- **length:** ``0x194``

## Header

- **offset:** ``0x00``
- **length:** ``0x08``

``` 
24 4F 36 06
A8 6E 49 06
```

## Mastery Entry

- **offset:** ``0x08``
- **length:** ``0x05``

```
!!                      // Mastery ID
!!                      // Mastery tree
03 00
!!                      // Points spent
```

- **Mastery ID** does not correlate with **talentId** in AIR Client
- **Mastery ID** contains the **coordinates** of the mastery in its tree.
  - First 4 bits contain the row (starting at ``0x04``)
  - Last 4 bits contain the column (starting at ``0x01``)
- **Mastery tree**
  - ``0x74`` = Offense
  - ``0x75`` = Defense
  - ``0x76`` = Utility
- **Points spent** referse to the number of points in specific mastery, with a maximum of ``0x04``

**Example: **

```
53 74 03 00 01
```

- Offensive tree
- Second row, third column => Mental Force (4123)
- 1 point spent

**Note:**
To get the **talentId** from this entry, one can use this formula: 
``(4100 + (secondByte - 0x74) * 0x64 + ((firstByte >> 4) - 0x03) * 0x0A + (firstByte & 0x0F)``

## Padding

```
00 00 00 00 ...
```

# Items 

- **offset:** ``0x2E3``
- **length:** ``N/A``

## Header

- **offset:** ``0x00``
- **length:** ``0x09``

```
1E XX B3 00 65 FE 00 0C 01
```

## Item Entries

- **offset:** ``0x09``
- **length:** ``0x07``

```
++ ++ 00 00             // Item ID
!!                      // Index (slot)
!!                      // Quantity
!!                      // Charges
```

- There are **9** item entries
- **Index** refers to the slot in which the item is placed, ranging from ``0x00`` to ``0x08``
- 8th and 9th item entries are always the same

**8th:**

```
D1 07 00 00             
07
01
00
```

- This **item ID** does not map to any item

**9th:**

```
00 00 00 00
08
00
00
```

## Unknown data (padding?)

- **offset:** ``N/A``
- **length:** ``N/A``

**Example:**

```
00 00 80 BF  
00 0E 8E 40 
00 00 80 BF
00 00 80 BF
00 00 80 BF
00 00 80 BF
00 00 80 BF
00 00 80 BF
00 00 80 BF
```

- Default value of each row is ``00 00 80 BF`` (-1.0)
  - Value of 2nd row can sometimes be ``00 0E 8E 40`` (4.439209)

# Unknown data

- **offset:** ``N/A``
- **length:** ``0x130``

Example:

```
A3 00 28 01
00 00 45 00 
28 01 00 00  // 296
06 00 00 00  // 6
00 00 00 00  // 0
07 00 00 00  // 7
0F 00 00 00  // 15
01 00 00 00  // 1
00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00
F8 26 1C 46  // 9993.742188
00 6C 02 46  // 8347
00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00
00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00
00 00 00 00  // 0
1F 00 00 00  // 31
03 00 00 00  // 3
93 85 30 43  // 176.521774
03 00 00 00  // 3
02 00 00 00  // 2
00 00 00 00  // 0
CB 0A 26 44  // 664.168640
08 A0 6C 45  // 3786.001953
C4 CC 7F 43  // 255.799866
CC 13 E5 45  // 7330.474609
17 00 00 00  // 23
00 00
4B 00 00 00  // 75
05 00 00 00  // 5
46 00 00 00  // 70
04 00 00 00  // 4
00 00 00 00  // 0
8B AA AF 47  // 89941.085938
48 23 42 46  // 12424.820312
CF 45 0A 46  // 8849.452148
00 00 00 00  00 00 00 00  00 00 00 00  00 00 00 00
3E 00 00 00  // 62
00 00 00 00  // 0
44 00 00 00  // 68
09 00 00 00  // 9
04 00 00 00  // 4
13 00 00 00  // 13
00 00 00 00  // 0
64 00 00 00  // 100
00 00 00 00  00 00 00 00  00 00 00 00
7B 7F D4 47  // 108798.960938
CB 6F 5D 46  // 14171.948242
EA 77 86 46  // 17211.957031
B1 09 00 00  // 2481
CD 04 BE 43  // 380.037506
18 4B 00 43  // 128.293335
01 00 00 00  // 1
00 00 00 00  // 0
12 80 6B 46  // 15072.017578
AD 6A BA 44  // 1491.333618
11 01 81 44  // 1032.033325
00 00 00 00  // 0
00 00 00 00  // 0
00 00 00 00  // 0
00 00 00 00  // 0
07 00 00 00  // 7
00 00 00 00  // 0
00 00 00 00  // 0
00 00
```

# Abilities

- **offset:** ``N/A``
- **length:** ``N/A``

## Header

- **offset:** ``0x00``
- **length:** ``0x04``

```
B3 00 03 15
```

## Ability entry

- **offset:** ``0x04``
- **length:** ``0x07``

```
00  
!!  //Ability number
!!  //Ability level
00 
!!  //Marker #1
00 
!!  //Marker #2
```

- **Ability number** refers to ability number, ranging from ``0x00`` to ``0x03``
- **Ability level** refers to ability level
- **Marker #1** is always ``0xF3``, except for the last ability, where it is ``0xB3``
- **Marker #2** is the same for each one, except for the last one

**Example:**

```
00 00 00 01  F3 00 03
00 01 00 01  F3 00 03
00 02 00 01  F3 00 03
00 03 00 01  B3 00 15
```

# String data

- **offset:** ``N/A``
- **length:** ``variable``

**Example:** 

```
17 00 2C 01
6C 75 63 69 61 6E 72 6D 69 73 73 69 6C 65 00 // "lucianrmissile"
F3 00 1E 00 32 01
63 72 65 73 74 6F 66 74 68 65 61 6E 63 69 65 6E 74 67 6F 6C 65 6D 6C 69 6E 65 73 00 // "crestoftheancientgolemlines"
F3 00 10 00 33 01
6C 75 63 69 61 6E 71 64 61 6D 61 67 65 00
```

**Example:**

```
17 00 01 01
76 69 77 00 // "viw"
F3 00 0A 00 2F 01
76 69 72 64 75 6E 6B 00 // "virdunk"
F3 00 17 00 32 01
69 74 65 6D 70 6C 61 63 65 6D 65 6E 74 6D 69 73 73 69 6C 65 00 // "itemplacementmissile"
```

- There is no known use for these strings

# Unknown data

- **offset:** ``N/A``
- **length:** ``variable``

**Example:**

```
B3 00  09 84 00 40  00 FE 4C 40  00 00 80
BF F3  00 09 00 43  C0 DD C5 41  00 00 80
BF F3  00 09 00 05  00 DC 9D 40  00 00 80
BF F3  00 09 00 0A  00 DA 1E 40  00 00 80
BF F3  00 09 00 40  D0 CE B8 42  00 00 80
BF F3  00 09 00 41  80 BB 1F 42  00 00 80
BF B3  00 01 EF 00  01 93 00 07  FE 00 00
```

**Example:**

```
B3 00  09 84 00 40  E0 85 2E 43  00 00 80
BF F3  00 09 00 41  58 B1 85 43  00 00 80
BF B3  00 01 EF 00  01 93 00 07  FE 00 00
```

- Many of these seem to be floats
- ``07 FE 00 00`` may signal end of section

# Footer

- **offset:** ``N/A``
- **length:** ``0x09``

```
00 00 15 01  
++ ++ ++ 40  //Entity ID
00
```

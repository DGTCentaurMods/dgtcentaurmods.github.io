---
layout: post
title:  "Board Fields Index"
author: Ed
categories: [ Centaur, Protocols ]
image: assets/images/4.jpg
---
# Board fields index

## How to convert field decimal value to hex and back:  
Field are called in funcftions using decimal values (A1 = 0 and H8 = 63) but the board electronics uses a different index (A8 down to H1). Use the below functions if you need to convert. 

### Hex to field value:
- convert hex value of the field into a decimal to use below  

Row  = field % 8  
Col = field // 8  
- then: 
`(7 - row) * 8 + col`

### Field value to hex: 
Row = field // 8  
Col = field % 8  
- then: 
`(7 - row) * 8 + col`
- convert final value to hex 

```
+-----+-----+-----+-----+-----+-----+-----+-----+
|  56 |  57 |  58 |  59 |  60 |  61 |  62 |  63 | 8
| x00 | x01 | x02 | x03 | x04 | x05 | x06 | x07 |
+-----+-----+-----+-----+-----+-----+-----+-----+
|  48 |  49 |  50 |  51 |  52 |  53 |  54 |  55 | 7
| x08 | x09 | x0a | x0b | x0c | x0d | x0e | x0f |
+-----+-----+-----+-----+-----+-----+-----+-----+
|  40 |  41 |  42 |  43 |  44 |  45 |  46 |  47 | 6
| x10 | x11 | x12 | x13 | x14 | x15 | x16 | x17 |
+-----+-----+-----+-----+-----+-----+-----+-----+
|  32 |  33 |  34 |  35 |  36 |  37 |  38 |  39 | 5
| x18 | x19 | x1a | x1b | x1c | x1d | x1e | x1f |
+-----+-----+-----+-----+-----+-----+-----+-----+
|  24 |  25 |  26 |  27 |  28 |  29 |  30 |  31 | 4
| x20 | x21 | x22 | x23 | x24 | x25 | x26 | x27 |
+-----+-----+-----+-----+-----+-----+-----+-----+
|  16 |  17 |  18 |  19 |  20 |  21 |  22 |  23 | 3
| x28 | x29 | x2a | x2b | x2c | x2d | x2e | x2f |
+-----+-----+-----+-----+-----+-----+-----+-----+
|  8  |  9  |  10 |  11 |  12 |  13 |  14 |  15 | 2
| x30 | x31 | x32 | x33 | x34 | x35 | x36 | x37 |
+-----+-----+-----+-----+-----+-----+-----+-----+
|  0  |  1  |  2  |  3  |  4  |  5  |  6  |  7  | 1
| x38 | x39 | x3a | x3b | x3c | x3d | x3e | x3f |
+-----+-----+-----+-----+-----+-----+-----+-----+
  A     B     C     D     E     F     G     H   
```

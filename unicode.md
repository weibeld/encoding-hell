---
title:  Unicode
author: Daniel Weibel
date:   May 2017
layout: page
---

Unicode maps **characters** (printable and non-printable ones) to identifiers called **code points**.

Here are some examples:

|Character |   Code Point |
|:--------:|-------------:|
|*\<LF\>*  |      `U+000A`|
|A         |      `U+0041`|
|a         |      `U+0061`|
|Ö         |      `U+00D6`|
|é         |      `U+00E9`|
|β         |      `U+03B2`|
|€         |      `U+20AC`|
|⅓         |      `U+2153`|
|∫         |      `U+222B`|
|⟶         |      `U+27F6`|
|貓        |      `U+8C93`|
|𝓦         |     `U+1D4E6`|
|🁗         |     `U+1F057`|
|🂵         |     `U+1F0B5`|
|🍰        |     `U+1F370`|
|😃        |     `U+1F603`|

A code point starts with `U+` and is followed by the code point's number which consists of four, five, or six hexadecimal digits.

The Unicode code points range from `0000` to `10FFFF` (1,114,111, uses 21 bits). Unicode is thus capable of representing **1,114,112 characters**.

This code point range is divided into **17 planes**:

|  Plane Nb   |     Start    |    End     |  Description  |
|------------:|-------------:|-----------:|:--------------|
|     0       |  `0000`      | `FFFF`     |   [Basic Multilingual Plane (BMP)](https://en.wikipedia.org/wiki/Plane_(Unicode)#Basic_Multilingual_Plane)|
|     1       |  `10000`     | `1FFFF`    |    [Supplementary Multilingual Plane (SMP)](https://en.wikipedia.org/wiki/Plane_(Unicode)#Supplementary_Multilingual_Plane)|
|     2       |  `20000`     | `2FFFF`    |    [Supplementary Ideographic Plane (SIP)](https://en.wikipedia.org/wiki/Plane_(Unicode)#Supplementary_Ideographic_Plane)|
|     3       |  `30000`     | `3FFFF`    |    unassigned|
|     4       |  `40000`     | `4FFFF`    |    unassigned|
|     5       |  `50000`     | `5FFFF`    |    unassigned|
|     6       |  `60000`     | `6FFFF`    |    unassigned|
|     7       |  `70000`     | `7FFFF`    |    unassigned|
|     8       |  `80000`     | `8FFFF`    |    unassigned|
|     9       |  `90000`     | `9FFFF`    |    unassigned|
|    10       |  `A0000`     | `AFFFF`    |    unassigned|
|    11       |  `B0000`     | `BFFFF`    |    unassigned|
|    12       |  `C0000`     | `CFFFF`    |    unassigned|
|    13       |  `D0000`     | `DFFFF`    |    unassigned|
|    14       |  `E0000`     | `EFFFF`    |    [Supplementary Special-Purpose Plane (SSP)](https://en.wikipedia.org/wiki/Plane_(Unicode)#Supplementary_Special-purpose_Plane)|
|    15       |  `F0000`     | `FFFFF`    |    [Supplementary Private Use Area-A (SPUA-A)](https://en.wikipedia.org/wiki/Plane_(Unicode)#Private_Use_Area_planes)|
|    16       |  `100000`    | `10FFFF`   |     [Supplementary Private Use Area-B (SPUA-B)](https://en.wikipedia.org/wiki/Plane_(Unicode)#Private_Use_Area_planes)|

Each plane consists of 65,536 (2^16) code points.

The most important plane is the **BMP**, which contains most characters of most of the scripts currently used in the world, mathematical symbols, and other signs. Its code points are specified with **four** hexadecmial digits.

The **SMP** contains characters of some historic scripts, calligraphic mathematical letters, and Emojis and other pictograms. Its code points are specified with **five** hexadecimal digits.

All the other planes are not really important for an average Joe. The **SPUA-B** plane is the only plane whose code points are represented with **six** hexadecimal digits.

> A Unicode character reference can be found [here](https://en.wikibooks.org/wiki/Unicode/Character_reference).

The first version of Unicode has been released in 1991.

All that Unicode does is assigning an identifier (a code point) to a character, for example, `U+0041` to `A`. However, Unicode does not define how `U+0041` is saved in memory. With 1 byte? With 2 bytes? With 3 bytes? With 4 bytes? With which bit pattern? All this depends on the used **character encodings**.





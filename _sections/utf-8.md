---
title:  UTF-8
author: Daniel Weibel
date:   May 2017
layout: page
---


As we have seen, there exist many different character encodings. Some of the most important ones are:

- [ASCII (American Standard Code for Information Interchange)](https://en.wikipedia.org/wiki/ASCII): released in 1963, assigns numbers between 0 and 127 to 128 characters, and encodes these numbers as the corresponding binary number in 8 bits. For example `A` got the number 65 (41 in hexadecimal) and is thus encoded as the 8-bit string `01000001`. Note that the most significant bit always remains 0. The 33 characters 0 to 31 and 127 are non-printable control characters (for example, *<line feed>*, *<backspace>*, *<end of transmission>*), and the 95 characters 32 to 126 are printable characters commonly used in the English language (letters, numbers, punctuation, etc.). ASCII was the base for many subsequent character encodings.
- [ISO 8859-1](https://en.wikipedia.org/wiki/ISO/IEC_8859-1): released in 1987, known as *Latin-1*, and part of the [ISO 8859 series of character encodings](https://en.wikipedia.org/wiki/ISO/IEC_8859). The ISO 8859 encodings are like ASCII 8-bit encodings, that is, they encode each character to an 8-bit string. ISO 8859-1 contains 191 characters, and thus, unlike ASCII, the most significant bit of encoded characters may be 1. ISO 8859-1 uses the ASCII mappings for the 95 printable ASCII characters (32 to 126), which means that it maps like ASCII `A` to `01000001`, `B` to `01000010`, and so on. On top of this, ISO 8859-1 adds accented latin characters used in Western-European languages, such as `É` (U+00C9) or `å` (U+00E5). ISO 8859-1 was the default encoding in HTML 4.
- [Windows-1252](https://en.wikipedia.org/wiki/Windows-1252): Microsoft's interpretation of ISO 8859-1 and used on Windows system in place of ISO 8859-1. Windows-1252 is identical with ISO 8859-1 except that it adds 27 printable characters that are not present in ISO 8859-1. These additional characters include curly quotation marks `“` (U+201C) and `”` (U+201D), curly apostrophes `‘` (U+2018) and `’` (U+2019), and the Euro sign `€` (U+20AC).

All of these encodings have in common that they map code points to 8-bit sequences. That means, each of these encodings can encode a maximum of 256 code points.

We have seen that Unicode defines a total of 1,114,112 code points. That means that each of the above encodings can encode only a very small subset of the Unicode code points.^[Note, though, that in Unicode 9.0 only [128,172](http://www.unicode.org/versions/Unicode9.0.0/) of the 1,114,112 code points are actually assigned to characters.]

The subsets of Unicode code points used by different encodings are different, and this easily leads to encoding/decoding misinterprations, as shown in the previous section.

There is a family of encodings which are specifically designed to handle *all* the 1,114,112 code points of Unicode. These are the Unicode Transformation Format (UTF) encodings.

The UTF encodings are said to **implement Unicode** since they allow to encode *all* the code points of Unicode.

The most prominent UTF encodings are:

- [UTF-8](https://en.wikipedia.org/wiki/UTF-8): uses 8, 16, 24, or 32 bits to encode a Unicode code point
- [UTF-16](https://en.wikipedia.org/wiki/UTF-16): uses 16 or 32 bits to encode a Unicode code point
- [UTF-32](https://en.wikipedia.org/wiki/UTF-32): always uses 32 bits to encode a Unicode code point

UTF-8 is by far the most widely used of these UTF encodings. At present it is used by [more than 89%](https://w3techs.com/technologies/history_overview/character_encoding) of websites in the Internet. Furthermore, UTF-8 is the default encoding in HTML 5.

UTF-8 was released in 1993.

UTF-8 is a **variable-width encoding**. That means, it encodes different code points to codes of different lengths. In UTF-8 the possible code lengths are 1, 2, 3, and 4 bytes.

The following table defines the UTF-8 encoding scheme:

| Min. Code Point|  Max. Code Point   |  Bit Pattern                             |  Nb. of Bits for Code Points  | Total Code Length |
|---------------:|-------------------:|:-----------------------------------------|:-----------------------------:|:-----------------:|
|       U+0000   |          U+007F    | 0xxxxxxx                                 |      7                        |                8  |
|       U+0080   |          U+07FF    | 110xxxxx 10xxxxxx                        |     11                        |               16  |
|       U+0800   |          U+FFFF    | 1110xxxx 10xxxxxx 10xxxxxx               |     16                        |               24  |
|      U+10000   |        U+10FFFF    | 11110xxx 10xxxxxx 10xxxxxx 10xxxxxx      |     21                        |               32  |

UTF-8 divides the Unicode code point space into four ranges that are encoded to 1, 2, 3, or 4 bytes, respectively.

In the following, we will go through the aboe table row by row.

#### Range 1: U+0000 to U+007F ⟶  encode to 1-byte code

This first range of code points has the following boundaries:


|                 | Min.      | Max.        |
|:----------------|----------:|------------:|
| **Hexadecimal** |         0 |          7F |
| **Decimal**     |         0 |         127 |
| **Binary**      |         0 |     1111111 |

This range includes all the code points that have **between 0 and 7 significant bits** in binary representation.

There are 128 code points in this range.

In general, UTF-8 encodes code points by inserting the binary representation of the code point number at the place of the `x`s in the corresponding bit pattern (the binary number is left-padded with 0s to the number of `x`s in the bit pattern, if necessary).

The bit pattern for the code point range 1 is **0**xxxxxxx. This means that in the resulting code, the leftmost bit is always 0, and the remaining 7 bits represent the code point number.

Let's see an example. the code point U+0041 (character `A`) is 1000001 in binary. This consists of 7 bits. These 7 bits are inserted at the place of the `x`s in **0**xxxxxxx, what results in **0**1000001.

So, the UTF-8 code for U+0041 is 01000001.

Let's take U+0021 (the exclamation mark `!`). The binary representation of this code point is 100001. In this case, it is only 6 bits long, so we left-pad it with 0s to the number of `x`s in the bit pattern (7), and we get 0100001. These 7 bits are then inserted into the bit pattern as in the previous example, and we get **0**0100001.

So, the UTF-8 code for U+0021 is 00100001.

If we look closely at the code point range U+0000 to U+007F, we see that these are exactly the 128 characters of the ASCII encoding. Furthermore, the UTF-8 codes for these characters are identical to the corresponding ASCII codes. This makes UTF-8 backwards-compatible with ASCII, or, in other words, valid ASCII encoded text is automatically also valid UTF-8 encoded text.

It was an important design goal for UTF-8 to make it backwards-compatible with ASCII.

#### Range 2: U+0080 to U+07FF ⟶  encode to 2-byte code

The second code point range has the following boundaries:


|                 | Min.      | Max.        |
|:----------------|----------:|------------:|
| **Hexadecimal** |        80 |         7FF |
| **Decimal**     |       128 |        2047 |
| **Binary**      |  10000000 | 11111111111 |

This range includes all the code points that have **between 8 and 11 significant bits** in binary representation.

There are **1920 code points** in this range.

Code points in this range are encoded to a 2-byte code with the bit pattern  **110**xxxxx **10**xxxxxx.

For multi-byte codes (that is, 2-byte, 3-byte, or 4-byte codes) the first byte is called the *leading byte* and it always starts with a sequence of leading 1s (in this case 2) followed by a 0. In fact, the number of leading 1s indicates the number of bytes in this multi-byte code, that is, whether it is a 2-byte, 3-byte, or 4-byte code.

The second code byte is a *continuation byte*. Continuation bytes always start with a single leading 1 followed by a 0.

Note that this approach allows to correctly identify each byte when decoding. Leading bytes and continuation bytes of multi-byte codes cannot be confused with single-byte codes, as single-byte codes never start with a 1. Leading bytes and continuation bytes also cannot be mutually confused, as continuation bytes have a single leading 1, whereas leading bytes always have at least 2 leading 1s.

Let's see an example. We want to encode the code point U+00E9 (character `é`). The binary representation of U+00E9 is 11101001. This number has 8 significant bits, but there are totally 11 `x`s in the bit pattern, so we pad the number with to 11 bits: 00011101001. Now we insert these bits at the place of the `x`s in **110**xxxxx **10**xxxxxx in the same order, and we get **110**00011 **10**101001.

So, the UTF-8 code for U+00E9 is 11000011 10101001.

Written in the more compact hexadecimal notation, this is `C3 A9`


#### Range 3: U+0800 to U+FFFF ⟶  encode to 3-byte code

The third code point range has the following boundaries:

|                 | Min.         | Max.             |
|:----------------|-------------:|-----------------:|
| **Hexadecimal** |          800 |             FFFF |
| **Decimal**     |         2048 |           65,535 |
| **Binary**      | 100000000000 | 1111111111111111 |

This range includes all the code points that have **between 12 and 16 significant bits** in binary representation.

There are **63,488 code points** in this range.

Code points in this range are encoded to a 3-byte code with the bit pattern  **1110**xxxx **10**xxxxxx **10**xxxxxx.

The bit pattern consists of a leading byte (with 3 leading 1s) and two continuation bytes, and has space for 16 code point bits.

Let's encode the code point U+0E28, which corresponds to the Thai character `ศ`. The binary representation of U+0E28 is 111000101000. This number has 12 significant bits, so we pad it to 16 bits: 0000111000101000. Then we insert these bits into the bit pattern, and we get **1110**0000 **10**111000 **10**101000.

So, the UTF-8 code of U+0E28 is 11100000 10111000.

Written in hexadecimal, this is `E0 B8 A8`.

Note that the code point ranges 1--3 together make up the [Basic Multilingual Plane (BMP)](https://en.wikipedia.org/wiki/Plane_(Unicode)#Basic_Multilingual_Plane) of Unicode (revisit the Unicode character reference [here](https://en.wikibooks.org/wiki/Unicode/Character_reference)). This means that *all* code points of the BMP (which sum up to 65,536) can be encoded as one, two, or three bytes in UTF-8.


#### Range 4: U+10000 to U+10FFFF ⟶  encode to 4-byte code

The fourth code point range has the following boundaries:

|                 | Min.              | Max.                  |
|:----------------|------------------:|----------------------:|
| **Hexadecimal** |             10000 |                10FFFF |
| **Decimal**     |            65,536 |             1,114,111 |
| **Binary**      | 10000000000000000 | 111111111111111111111 |

This range includes all the code points that have **between 17 and 21 significant bits** in binary representation.

There are **1,048,576 code points** in this range.

This range includse all the Unicode code points that are not in the Basic Multilingual Plane (BMP), which corresponds to all the code points in the Unicode planes 1--16.

Code points in this range are encoded to a 4-byte code with the bit pattern  **11110**xxx **10**xxxxxx **10**xxxxxx **10**xxxxxx.

The bit pattern consists of a leading byte (with 4 leading 1s) and three continuation bytes, and has space for 21 code point bits.

Actually, it's in this range that encodings becomes real fun, because this range includes the emoji code points (in the [Supplementary Multilingual Plane (SMP)](https://en.wikipedia.org/wiki/Plane_(Unicode)#Supplementary_Multilingual_Plane)). Let's enccode code point U+1F603, which corresponds to the emoji 😃. The binary representation of U+1F603 is 11111011000000011. This number has 17 significant bits. We pad the number to 21 bits, insert these bits into the bit pattern, and we get **11110**000 **10**011111 **10**011000 **10**000011.

So, the UTF-8 encoding of U+1F603 is 11110000 10011111 10011000 10000011.

Written in hexadecimal, this is `F0 9F 98 83`.


#### Example summary

Here is a summary of our example UTF-8 encodings:

| Character | Unicode Code Point  | UTF-8 Code Binary                      |  UTF-8 Code Hexadecimal|
|:---------:|--------------------:|:---------------------------------------|:-----------------------|
|  A        |        U+0041       |    011111                              |   41                   |
|  é        |        U+00E9       |    11000011 10101001                   |   C3 A9                |
|  ศ        |        U+0E28       |    11100000 10111000 10101000          |   E0 B8 A8             |
| 😃        |       U+1F603       |    11110000 10011111 10011000 10000011 |   F0 9F 98 83          |

Encoding *any* Unicode code point with UTF-8 boils down to determine in which range the code point is and then doing the approprate conversion as described above.

So, now you know how to convert each of 1,114,112 Unicode code points to a binary signal with UTF-8. Great! 👏 

Let's look at the codes produced by UTF-8 a moment. So, UTF-8 may produce multi-byte codes like `C3 A9` for a character like `é`... In the previous section we have seen that there are popular encodings which use only single-byte codes... So, what if somebody attempted to decode the UTF-8 code `C3 A9` with, say, ISO 8859-1? Your mojibake alarm bells should ring!

In the next section we look at mojibakes that can occur when UTF-8 is involved.


## More Mojibake

There are basically two scenarios in which mojibakes with UTF-8 can occur:

1. A text is **encoded with a single-byte encoding** (e.g ISO 8859-1) and **decoded with UTF-8**
    - In this case, we are likely to see characters like `�`
2. A text is **encoded with UTF-8** and **decoded with a single-byte encoding** (e.g. ISO 8859-1)
    - In this case, you are likely to see garbled character sequences like `ï¿½`


#### Encode with a non-UTF-8 encoding ⟶  decode with UTF-8

If we encode the name of the Spanish province `Aragón` with ISO 8859-1, and then decode it with UTF-8, we get `Arag�n`.

Let's see step by step what's going on here:

~~~
                    ISO 8859-1 (Latin-1)                                 UTF-8
                 +-------------------------+                  +-------------------------+
A (U+0041) ====> |                         | ==> 01000001 ==> |                         | ====> A (U+0041)
r (U+0072)       | code point     bits     |     01110010     | code point     bits     |       r (U+0072)
a (U+0061)       | ----------------------- |     01100001     | ------------------------|       a (U+0061)
g (U+0066)       |                         |     01100110     |                         |       g (U+0066)
ó (U+00F3)       | U+0041 (A) <=> 01000001 |     11110011     | U+0041 (A) <=> 01000001 |       � (U+FFFD)
n (U+006E)       | U+0061 (a) <=> 01100001 |     01101110     | U+0061 (a) <=> 01100001 |       n (U+006E)
                 | U+0066 (g) <=> 01100110 |                  | U+0066 (g) <=> 01100110 |
                 | U+006E (n) <=> 01101110 |                  | U+006E (n) <=> 01101110 |
                 | U+0072 (r) <=> 01110010 |                  | U+0072 (r) <=> 01110010 |
                 | ...              ...    |                  | ...            ...      |
                 | U+00F3 (ó) <=> 11110011 |                  |                         |
                 |                         |                  +-------------------------+
                 +-------------------------+                  
~~~

First we encode the code point sequence corresponding to `A`, `r`, `a`, `g`, `ó`, `n` with ISO 8859-1. This results in a 6-byte-long code.

Then we start decoding these 6 bytes with UTF-8. For the bytes corresponding to `A`, `r`, `a`, `g` all works well, UTF-8 decodes them to the correct code points. This is because the characters read so far are pure ASCII characters, and ISO 8859-1 and UTF-8, as well as ASCII itself, encode the ASCII characters to exactly the same bit sequences. So, as far as only ASCII characters are concerned, there is no communication problem between all these encodings.

However, things start to go wrong when it comes to the byte `11110011`, which represents the ISO 8859-1 code of `ó` (U+00F3). For UTF-8 this byte looks like a leading byte of a 4-byte code, because it starts with four leading 1s followed by a 0. So, UTF-8 checks the next byte, which it expects to be a continuation byte. However, continuation bytes always start with a single leading 1 followed by a 0, and the next byte `01101110` starts with a 0. So, UTF-8 concludes that this cannot be a 4-byte code and that the byte `11110011` is invalid and unrecognisable.

What UTF-8 does in such cases is to replace the invalid bytes in the input with the special Unicode code point U+FFFD. This code point is called the [replacement character](https://en.wikipedia.org/wiki/Specials_(Unicode_block)#Replacement_character), depicted as �, and defined for exactly this case of indicating invalid input data.

The decoding then goes on with the byte `01101110`, which is a valid UTF-8 code for the code point U+006E (letter `n`).

So, the output of UTF-8 is the code point sequence (U+0041, U+0072, U+0061, U+0066, U+FFFD, U+0064), which corresponds to the string `Arag�n`.

#### Encode with UTF-8 ⟶  decode with another encoding

In the previous section we have seen that if we decode a text with UTF-8 that has not been encoded with UTF-8, we may get the replacment character � in the output.

Now, let's look at the reverse case: we encode a text with UTF-8 and decode it with another encoding.


**Example 1: résumé ⟶  rÃ©sumÃ©**





~~~
                               UTF-8                                      ISO 8859-1 (Latin-1) 
                 +----------------------------------+                  +-------------------------+
r (U+0072) ====> |                                  | ==> 01110010 ==> |                         | ====> r (U+0072)
é (U+00E9)       | code point     bits              |     11000011     | code point     bits     |       Ã (U+00C3)
s (U+0073)       | -------------------------------- |     10101001     | ----------------------- |       © (U+00A9)
u (U+0075)       |                                  |     01110011     |                         |       s (U+0073)
m (U+006D)       | U+006D (m) <=> 01101101          |     01110101     | U+006D (m) <=> 01101101 |       u (U+0075)
é (U+00E9)       | U+0072 (r) <=> 01110010          |     01101101     | U+0072 (r) <=> 01110010 |       m (U+006D)
                 | U+0073 (s) <=> 01110011          |     11000011     | U+0073 (s) <=> 01110011 |       Ã (U+00C3)
                 | U+0075 (u) <=> 01110101          |     10101001     | U+0075 (u) <=> 01110101 |       © (U+00A9)
                 | ...            ...               |                  | ...            ...      |
                 | U+00E9 (é) <=> 11000011 10101001 |                  | U+00A9 (©) <=> 10101001 |
                 |                                  |                  | U+00C3 (Ã) <=> 11000011 |
                 +----------------------------------+                  |                         |
                                                                       +-------------------------+
~~~

More accented character mojibakes: 

| Character |   Code point|        Binary UTF-8 code | Hexadecimal UTF-8 code   |     ISO 8859-1 decoded |
|:---------:|:-----------:|:------------------------:|:------------------------:|:------------------------:|
| à         |      U+00E0 |      11000011 10100000   |       C3 A0              |          Ã*[[NBSP]](https://en.wikipedia.org/wiki/Non-breaking_space)*|
| á         |      U+00E1 |      11000011 10100001   |       C3 A1              |          Ã¡|
| â         |      U+00E2 |      11000011 10100010   |       C3 A2              |          Ã¢|
| ã         |      U+00E3 |      11000011 10100011   |       C3 A3              |          Ã£|
| ä         |      U+00E4 |      11000011 10100100   |       C3 A4              |          Ã¤|
| å         |      U+00E5 |      11000011 10100101   |       C3 A5              |          Ã¥|
| ç         |      U+00E7 |      11000011 10100111   |       C3 A7              |          Ã§|
| è         |      U+00E8 |      11000011 10101000   |       C3 A8              |          Ã¨|
| é         |      U+00E9 |      11000011 10101001   |       C3 A9              |          Ã©|
| ê         |      U+00EA |      11000011 10101010   |       C3 AA              |          Ãª|
| ë         |      U+00EB |      11000011 10101011   |       C3 AB              |          Ã«|
| ì         |      U+00EC |      11000011 10101100   |       C3 AC              |          Ã¬|
| í         |      U+00ED |      11000011 10101101   |       C3 AD              |          Ã*[[SHY]](https://en.wikipedia.org/wiki/Soft_hyphen)*|
| î         |      U+00EE |      11000011 10101110   |       C3 AE              |          Ã®|
| ï         |      U+00EF |      11000011 10101111   |       C3 AF              |          Ã¯|
| ñ         |      U+00F1 |      11000011 10110001   |       C3 B1              |          Ã±|
| ò         |      U+00F2 |      11000011 10110010   |       C3 B2              |          Ã²|
| ó         |      U+00F3 |      11000011 10110011   |       C3 B3              |          Ã³|
| ô         |      U+00F4 |      11000011 10110100   |       C3 B4              |          Ã´|
| õ         |      U+00F5 |      11000011 10110101   |       C3 B5              |          Ãµ|
| ö         |      U+00F6 |      11000011 10110110   |       C3 B6              |          Ã¶|
| ø         |      U+00F8 |      11000011 10111000   |       C3 B8              |          Ã¸|
| ù         |      U+00F9 |      11000011 10111001   |       C3 B9              |          Ã¹|
| ú         |      U+00FA |      11000011 10111010   |       C3 BA              |          Ãº|
| û         |      U+00FB |      11000011 10111011   |       C3 BB              |          Ã»|
| ü         |      U+00FC |      11000011 10111100   |       C3 BC              |          Ã¼|


**Example 2: “Let’s go” ⟶  â€œLetâ€™s goâ€?**

This is one for Windows users. Imagine somebody types the phrase `"Let's go"` in a Microsoft text processing program, and the program's "Smart Quotes" function replaces the straight double quotes and apostroph with curly double quotes and a curly apostroph, respectivtly (namely `“` (U+201C), `”` (U+201D), and `’` (U+2019)). The user then saves the file to disk using UTF-8 encoding. If the file is later opened with an editor that uses the Windows-1252 encoding (the equivalent to ISO 8859-1 on Windows), the phrase will read **â€œLetâ€™s goâ€?**.

Now, this looks really garbled!

Let's see what's going on here:

~~~
                               UTF-8                                              Windows-1252 
                 +-------------------------------------------+                  +-------------------------+
“ (U+201C) ====> |                                           | ==> 11100010 ==> |                         | ====> â (U+00E2)
L (U+004C)       | code point     bits                       |     10000000     | code point     bits     |       € (U+20AC)
e (U+0065)       | ----------------------------------------- |     10011100     | ----------------------- |       œ (U+0153)
t (U+0074)       |                                           |     01001100     |                         |       L (U+004C)
’ (U+2019)       | U+0020 ( ) <=> 00100000                   |     01100101     | U+0020 ( ) <=> 00100000 |       e (U+0065)
s (U+0073)       | U+004C (L) <=> 01001100                   |     01110100     | U+004C (L) <=> 01001100 |       t (U+0074)
  (U+0020)       | U+0065 (e) <=> 01100101                   |     11100010     | U+0065 (e) <=> 01100101 |       â (U+00E2)
g (U+0067)       | U+0067 (g) <=> 01100111                   |     10000000     | U+0067 (g) <=> 01100111 |       € (U+20AC)
o (U+006F)       | U+006F (o) <=> 01101111                   |     10011001     | U+006F (o) <=> 01101111 |       ™ (U+2122)
” (U+201D)       | U+0073 (s) <=> 01110011                   |     01110011     | U+0073 (s) <=> 01110011 |       s (U+0073)
                 | U+0074 (t) <=> 01110100                   |     00100000     | U+0074 (t) <=> 01110100 |         (U+0020)
                 | ...            ...                        |     01100111     | ...            ...      |       g (U+0067)
                 | U+2019 (’) <=> 11100010 10000000 10011001 |     01101111     | U+00E2 (â) <=> 11100010 |       o (U+006F)
                 | U+201C (“) <=> 11100010 10000000 10011100 |     11100010     | U+0153 (œ) <=> 10011100 |       â (U+00E2) 
                 | U+201D (”) <=> 11100010 10000000 10011101 |     10000000     | U+20AC (€) <=> 10000000 |       € (U+20AC)
                 |                                           |     10011101     | U+2122 (™) <=> 10011001 |       ?
                 +-------------------------------------------+                  |                         |
                                                                                +-------------------------+
~~~

As we can see above, UTF-8 encodes the curly quotes and the curly apostrop to three bytes each. All the other characters are regular ASCII characters and are encoded to one byte each. This creates a 16-byte UTF-8 code for the 10-character phrase `“Let’s go”`.

Windows-1252 on the other hand interprets each of these 16 bytes as an individual character. For the bytes corresponding to ASCII characters, this produces the right result, as UTF-8 and Windows-1252 (as well as ASCII itself) are compatible with respect to ASCII characters. For the 3-byte codes corresponding to the curly quotes and apostroph, however, Windows-1252 outputs for each byte a character that just happens to be mapped to the bit pattern of this byte.

For the first two bytes of each of these 3-byte codes, these are the characters `â` and `€`, as the three codes differ only in their last byte. For the last byte of the opening curly quote `“`, it is `œ`, and for the last byte of the curly apostroph `’`, it is `™`. Finally, for the last byte `10011101` of the closing curly quote, Windows-1252 does not have a mapping, and it treats it as an invalid unrecognised character and outputs an appropriate indicator, such as a question mark, at its place.

**Example 3: Arag�n ⟶  Aragï¿½n**

~~~
                                UTF-8                                               ISO 8859-1 (Latin-1) 
                 +-------------------------------------------+                   +-------------------------+
A (U+0041) ====> |                                           | ==> 01000001 ==>  |                         | ====> A (U+0041)
r (U+0072)       | code point     bits                       |     01110010      | code point     bits     |       r (U+0072)
a (U+0061)       | ----------------------------------------- |     01100001      | ----------------------- |       a (U+0061)
g (U+0066)       |                                           |     01100110      |                         |       g (U+0066)
� (U+FFFD)       | U+0041 (A) <=> 01000001                   |     11101111      | U+0041 (A) <=> 01000001 |       ï (U+00EF)
n (U+006E)       | U+0061 (a) <=> 01100001                   |     10111111      | U+0061 (a) <=> 01100001 |       ¿ (U+00BF)
                 | U+0066 (g) <=> 01100110                   |     10111101      | U+0066 (g) <=> 01100110 |       ½ (U+00BD)
                 | U+006E (n) <=> 01101110                   |     01101110      | U+006E (n) <=> 01101110 |       n (U+006E)
                 | U+0072 (r) <=> 01110010                   |                   | U+0072 (r) <=> 01110010 |
                 | ...            ...                        |                   | ...              ...    |
                 | U+FFFD (�) <=> 11101111 10111111 10111101 |                   | U+00BD (½) <=> 10111101 |
                 |                                           |                   | U+00BF (¿) <=> 10111111 |
                 +-------------------------------------------+                   | U+00EF (ï) <=> 11101111 |
                                                                                 |                         |
                                                                                 +-------------------------+
~~~


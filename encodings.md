---
title:  Character Encodings
author: Daniel Weibel
date:   May 2017
layout: page
---

A character encoding maps code points to bit patterns and vice versa:

~~~
                           Character Encoding
                    +------------------------------+
A (U+0041) =======> |                              | =======> 01000001
                    |  code point      bit pattern |
                    |  --------------------------- |
                    |                              |
                    |  ...              ...        |
                    |  @ (U+0040)  <=>  01000000   |
                    |  A (U+0041)  <=>  01000001   |
                    |  B (U+0042)  <=>  01000010   |
                    |  C (U+0043)  <=>  01000011   |
                    |  ...              ...        |
A (U+0041) <======= |                              | <======= 01000001
                    +------------------------------+
~~~

A character encoding has a set of code points that it "knows", and a corresponding bit pattern for each code point.

In the above example, each code point that the encoding knows is mapped to an 8-bit pattern.

There exist many different encodings. The following holds:

- Different encodings may know diverging sets of code points (that is, one encoding may know a code point that another encoding doesn't know).
- Different encodings may translate the same code point to different bit patterns.
- Different encodings may use the same bit pattern to represent different code points.

If the same encoding is used to both encode and decode a text, then everything works fine.

If different encodings are used, then strange things may happen.

Let's see some examples:

~~~
                      ISO 8859-1 (Latin-1)                              ISO 8859-11 (Thai)    
                 +-----------------------------+                  +-----------------------------+
ä (U+00E4) ====> |                             | ==> 11100100 ==> |                             | ====> ไ (U+0E44)
                 | code point      bit pattern |                  | code point      bit pattern | 
                 | --------------------------- |                  | --------------------------- |
                 |                             |                  |                             |
                 | ...              ...        |                  | ...              ...        |
                 | ä (U+00E4)  <=>  11100100   |                  | ไ (U+0E44)  <=>  11100100   |
                 | ...              ...        |                  | ...              ...        |
                 |                             |                  |                             |
                 +-----------------------------+                  +-----------------------------+
~~~

Both of these encodings maps code points to 8-bit strings.

In this example, the character `ä` (code point U+00E4) is encoded with the [ISO 8859-1](https://en.wikipedia.org/wiki/ISO/IEC_8859-1) encoding scheme (also known as Latin-1), resulting in the 8-bit string `11100100`. This bit string is then decoded with [ISO 8859-11](https://en.wikipedia.org/wiki/ISO/IEC_8859-11), which is an encoding scheme used to represent Thai characters. Now, ISO 8859-11 does not know the code point U+00E4 (character `ä`) at all, but instead uses the bit string `11100100` (which is used by ISO 8859-1 to represent U+00E4) to represent the code point U+0E44, which is the Thai character `ไ`. Hence, a text encoded with ISO 8859-1 will have all occurrences of `ä` replaced with `ไ` if decoded with ISO 8859-11.

By convention, most encoding schemes know the code points U+0020 to U+007E (32 to 126 in decmial) which are the most common printable characters in the English language. They furthermore map these specific code points to the same bit patterns. These mappings actually coincide with the mappings used by [ASCII](https://en.wikipedia.org/wiki/ASCII), one of the earliest and most influential character encoding shemes. For example, they all map `A` (U+0041) to  `01000001`, `B` (U+0042) to `01000010`, and so on. This is a pure convention, but it allows to correctly encode and decode texts that contain only characters from within this range across encoding schemes. Only characters that are outside of this code point range (U+0020 to U+007E) are at the risk of being misinterpreted.

If for example, the English word `bear` is encoded with ISO 8859-1, it would be correctly decoded to `bear` with ISO 8859-11.

On the other hand, if the German word `Bär` is encoded with ISO 8859-1, it would be incorrectly decoded with ISO 8859-11 to `Bไr`.

What if an encoding encounters a bit pattern that it cannot map to any code point?

~~~
                      ISO 8859-1 (Latin-1)                              ISO 8859-11 (Thai)    
                 +-----------------------------+                  +-----------------------------+
ü (U+00FC) ====> |                             | ==> 11111100 ==> |                             | ====> <?>
                 | code point      bit pattern |                  | code point      bit pattern | 
                 | --------------------------- |                  | --------------------------- |
                 |                             |                  |                             |
                 | ...              ...        |                  | ...              ...        |
                 | ü (U+00FC)  <=>  11111100   |                  |                             |
                 | ...              ...        |                  +-----------------------------+
                 |                             |
                 +-----------------------------+
~~~

In this example, the code point U+00FC (character `ü`) is encoded with ISO 8859-1 to the 8-bit string `11111100`, and we attempt to decode this bit string with ISO 8859-11. However, ISO 8859-11 does not have any code point associated with the bit string `11111100`. In this case, what ISO 8859-11 will most likey do is to replace the unknown code point with the one for a question mark (U+003F) or some other symbol and then go on with the decoding.

So, the string `München` encoded with ISO 8859-1 would be decoded by ISO 8859-11 as `M?nchen`.


Let's see another example:

~~~
                          Windows-1252                                 ISO 8859-1 (Latin-1)     
                 +-----------------------------+                  +-----------------------------+
“ (U+201C) ====> |                             | ==> 10010011 ==> |                             | ====> <?>
h (U+0068)       | code point      bit pattern |     01101000     | code point      bit pattern |       h (U+0068)
i (U+0069)       | --------------------------- |     01101001     | --------------------------- |       i (U+0069)
” (U+201D)       |                             |     10010100     |                             |       <?>
                 | ...              ...        |                  | ...              ...        |
                 | h (U+0068)  <=>  01101000   |                  | h (U+0068)  <=>  01101000   | 
                 | i (U+0069)  <=>  01101001   |                  | i (U+0069)  <=>  01101001   |
                 | ...              ...        |                  | ...              ...        |
                 | “ (U+201C)  <=>  10010011   |                  |                             |
                 | ” (U+201D)  <=>  10010100   |                  +-----------------------------+
                 | ...              ...        |
                 |                             |
                 +-----------------------------+
~~~

The [Windows-1252](https://en.wikipedia.org/wiki/Windows-1252) encoding is [Microsoft's interpretation](https://en.wikipedia.org/wiki/Windows_code_page) of the ISO 8859-1 encoding, and is used on Windows. Windows-1252 is identical to ISO 8859-1 except that it contains some code points that IOS 8859-1 does not contain. These code points include the curly quotation marks `“` (U+201C) and `”` (U+201D).

Now, people usually don't literally type in the curly quotes `“` and `”`, but just use the straight quotes `"`. However, some text processing software on Windows, such as Microsoft Word, has a so-called "Smart Quotes" function which automatically replaces straight quotes by the apprpriate curly quotes: `“` (U+201C) at the beginning of a word, and `”` (U+201D) at the end of a word. So, a string like `"hi"` becomes `“hi”`.

If the string `“hi”` is encoded with Windows-12512 (for example, on a Windows system), and then decoded with ISO 8859-1 (for example, on a macOS system), then ISO 8859-1 does not recognise the bytes `10010011` and `10010100` representing `“` and `”`, respectively, and replaces them with a replacement character like a question mark or a box. So, the original text `“hi”` would be decoded as `?hi?`.

This error may occur when a Windows-1252 encoded text file is mislabeled as ISO 8859-1 encoded and the text processing system thus decodes the file with ISO 8859-1.


| Character|Unicode Code Point | Windows-1252 Bit Pattern  |
|:--------:|------------------:|--------------------------:|
|“         |             U+201C|                         93|
|”         |             U+201D|                         94|
|„         |             U+201E|                         84|
|‟         |             U+201F|                          -|
|‘         |             U+2018|                         91|
|’         |             U+2019|                         92|
|‚         |             U+201A|                         82|
|‛         |             U+201B|                          -|


The quintessence of all this is:

> Any text should be **encoded and decoded** with the **same character encoding**

That's why every text file should be labelled in some way with the encoding scheme in which it was encoded so that the appropriate encoding scheme can be selected for the decoding.

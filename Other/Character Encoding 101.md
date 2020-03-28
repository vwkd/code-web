# Character Encoding 101

[TOC]

## Why do we need character encoding?

- computers can store only bits („binary digits“, 0 and 1)
- to represent anything else like letters, numbers, symbols needs character encoding scheme
- everything, e.g. text, file, image, is encoded, there is nothing like „plain text“, must always know the encoding standard used, otherwise the bits are useless
- seeing garbled text just means the wrong encoding was used when reading file, can decode only with proper encoding, unless if text is read using wrong encoding and then converted into another encoding, then information is lost unless exact history of conversion is known
- must always specify encoding used, e.g. for text, input, output



## Terminology

- character: minimal unit of text with semantic value, e.g. a letter
- charset, character set: a set of characters, e.g. the English language
- (character) encoding (standard): unique mapping of characters to numbers, e.g. ASCII
- coded character set: a charset for which an encoding is specified
- code space: set of numbers to which coded character set is mapped
- code point: element of code space, i.e. a number



## Encoding standards

### ASCII
- ASCII („American Standard Code for Information Interchange“) was first character encoding standard for english language, created in 1960s
- included 128 essential characters, e.g. `A` was encoded as `65`
- used 7 bits in binary for each code point, stored in 8-bit bytes
- soon world used additional 128 characters by making use of the 8th bit to create characters for their own languages
- lots of encodings were invented, basically for every keyboard
- some even used multiple bytes for one character
- different encodings worked more or less until the internet came along and information needed to be exchanged, then shit hit the fan
- one number in code space would correspond to all different characters depending on the encoding used, multiple bytes were read as distinct code points resulting in totally different characters, etc.
- information exchange became a mess

### Unicode

- one big coded character set to include all and every character on earth
- includes almost 138,000 characters as of May 2019 and grows constantly
- no need for any encoding conversion anymore, Unicode all the things!
- first 128 characters are same as ASCII
- not a character encoding standard, just the coded character set
- many different character encodings can encode all Unicode characters, like UTF-8, UTF-16, UTF-32
- a character in Unicode is notated by `U+XXXX`, where `XXXX` is the code point in hexadecimal, e.g. `A` is `U+0041`
- Unicode characters are grouped in "planes" of 2^16^ (65,536) code points, there are already multiple planes, since they come one after each other and are not yet full, some characters have code points larger than 16^4^ (65,536), those need more than 4 hexadecimal digits to be represented, i.e. `U+XXXXX` or `U+XXXXXX`

#### UTF-32

- fixed-length Unicode character encoding
- uses 4 bytes (= _32_ bits) per character
- even for most simple characters wastes 4 bytes

#### UTF-16
- variable-length Unicode character encoding
- uses 2 or 4 bytes (= _16_ or 32 bits) per character

#### UTF-8
- variable-length Unicode character encoding
- uses 1, 2, 3 or 4 bytes (= _8_, 16, 24, 32 bits) per character
- is the character encoding standard of the Web
- backwards compatible with ASCII, encodes first 128 characters with same binary values like ASCII, so ASCII encoded text is valid UTF-8 encoded text, vice versa since can use UTF-8 with most old programs and languages that expect ASCII, as long as no string manipulation (operation on character-level like slicing, trimming, counting) and just simple input and output no attention needed



## References

- [What Every Programmer Absolutely, Positively Needs To Know About Encodings And Character Sets To Work With Text](http://kunststube.net/encoding/) - David Zentgraf
- [The Absolute Minimum Every Software Developer Absolutely, Positively Must Know About Unicode and Character Sets (No Excuses!)](https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/) - Joel Spolsky
- [Character encoding](https://en.wikipedia.org/wiki/Character_encoding) - Wikipedia
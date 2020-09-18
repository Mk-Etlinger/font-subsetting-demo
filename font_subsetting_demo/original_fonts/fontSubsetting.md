# Font Subsetting:
===
## How to increase performance by *reducing* file size

---

## Unicode: Nothing To Fear
> Unicode provides a unique number for every character, no matter what the platform, no matter what the program, no matter what the language.

- www.unicode.org

---

===

Fundamentally, computers deal with numbers. 

They store letters and other characters by assigning a number for each one. 

Before the Unicode standard was developed, there were many different systems, called character encodings, for assigning these numbers.

---

### Unicode includes characters from most of today’s languages: 

* punctuation marks: , . '' "" ; :
* diacritics:  ø é ö â
* mathematical symbols: + - * % < >
* technical symbols: ⌀ ⌖ ⌆ ⍀ ⍼ ⍨
* arrows: → ⟵ ➢ ⇔
* emoji - 🤩 🤖 🤠 🥺 🤦
* and more!

[.build-lists: true]

---

## Quick overview on Hex Codes
Hexadecimal is a base-16 number system. 

That means there are 16 possible digits used to represent numbers. 

10 of the values are commonly used in decimal numbers:

* 0, 1, 2, 3, 4, 5, 6, 7, 8, and 9
* A, B, C, D, E, and F (maps to 10, 11, 12, 13, 14, and 15).

---

## UTF

The most common encoding for unicode is UTF-8 (Unicode Transformation Unit).

Here's an example of Hello in UTF-8:

```
48  65  6C  6C  6F
H     e     l      l      o
```

Prefixed versions: U+0048(H), U+0065(e), U+006C(l), U+006C(l), U+006F(o)

---

## Words to know:
**Abstract character**: (or character) is a unit of information used for the organization, control, or representation of textual data.

Every abstract character has an associated name, e.g. LATIN SMALL LETTER A. 

The rendered form (glyph) of this character is *a*.

---

===
**Grapheme**: or symbol, is a minimally distinctive unit of writing in the context of a particular writing system.

A grapheme is how a user thinks about a character. 

A concrete image of a grapheme displayed on the screen is named *glyph*.

---

===
**Code point**: is a number assigned to a single character.

Code points are numbers in the range from U+0000 to U+10FFFF.

---

![inline](https://dmitripavlutin.com/static/16d7bd44cac07b727121315ae7db1ab6/da8b6/unicode-terms.png)

---

## The Problem
Font files are actually quite large when including all possible glyphs

In the unicode standard there are *1,114,112* code points(the range U+0000 to U+10FFFF) 

*830,606* code points are considered reserved, but only 283,506* have assigned characters.

*(as of March 2020)

---

### In total, the Unicode standard supports *154 languages*

Can you imagine if our font files contained 283,506 glyphs?

That would absolutely wreck performance and kill bandwidth for our users...

source: **[https://www.unicode.org/standard/supported.html](https://www.unicode.org/standard/supported.html)**

---

## But have you looked at the font file sizes websites serve lately?
![inline](~/Desktop/cnbc_font_network_tab.png)

* 236 kb in total!!

[.build-lists: true]

---

### That's larger than the entire Motortrend application.

If CNBC serves 120,000,000 users a month:

* That's 28320 Gigabytes served over the wire
* !! ADD AN ADDITIONAL STAT HERE
* That's XXXXXX in energy which could power XXXXX lights for XXXX time

[.build-lists: true]

---

## Font Subsetting to the rescue!
![right autoplay](https://media.giphy.com/media/J2zwN64xc4wgw/giphy.gif)

This is what we look like when we try to serve 236kb of fonts to our users

---

## What is Font Subsetting?

> Font subsets are created to provide only a minimal number of glyphs for the character set coverage, thereby reducing the redundancy of font data transmission and reducing the total number of characters transmitted.

One of the fundamental metrics of performance is to serve only what is immediately needed to the user.

Font Subsetting does exactly that.

---

## Looking to the network tab
Let's take a look in the network tab at the response headers for a second.

I see that our document is gzipped, but our fonts are not. 

Why is that?

---

## Fonts are *ALREADY* compressed with their own compression
WOFF*, and WOFF2 are already using their own built in font compression.

EOT and TTF formats are not compressed by default. Ensure that your servers are configured to apply GZIP compression when delivering these formats.

*Web Open Font Format

---

## Now the fun part
Let's take a look at how we can start subsetting fonts today.

---

## @font-face { unicode-range: ... }

> The descriptor value is a comma-delimited list of Unicode range values. The union of these ranges defines the set of codepoints that serves as a hint for user agents when deciding whether or not to download a font resource for a given text run.

* **[https://drafts.csswg.org/css-fonts-3/#unicode-range-desc](https://drafts.csswg.org/css-fonts-3/#unicode-range-desc)**

---

===
In other words, the browser knows what glyphs it needs to render and references `unicode-range` to see if the needed glyphs matches. 

If it matches the `unicode-range`, the fonts are downloaded. 

Think of it like lazy loading for fonts.

---

---

---

---

---

---

---
 
## Caveats & Legal
* Tooling is lacking in JavaScript so Python is the easiest to use currently
* [https://caniuse.com/?search=unicode-range](https://caniuse.com/?search=unicode-range)
* You need to read the font license in depth and preferably run it by legal
* Some licenses prevent the use of the Reserved Font Name, so omit the original font name if you modify it in any way

---

## Sources

* https://www.unicode.org/
* https://dmitripavlutin.com/what-every-javascript-developer-should-know-about-unicode/#2-basic-unicode-terms
* https://wakamaifondue.com/
* https://github.com/Munter/subfont
* https://github.com/filamentgroup/glyphhanger

---

---

---
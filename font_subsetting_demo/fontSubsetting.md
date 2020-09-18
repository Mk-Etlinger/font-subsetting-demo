# Font Subsetting:
===
## How to increase performance by *reducing* file size

---

## Words to know:
**Abstract character**: (or character) is a unit of information used for the organization, control, or representation of textual data.

---

===
**Char Name**: In Unicode every abstract character has an associated name, e.g. LATIN SMALL LETTER A. 

The rendered form of this character is *a*.

---

===
**Grapheme**: or symbol, is a minimally distinctive unit of writing in the context of a particular writing system.

A grapheme is how a user thinks about a character.

---

===
**Glyph**: A rendered image of a grapheme.

---

===
**Code point**: is a number assigned to a single character.

Code points are numbers in the range from U+0000 to U+10FFFF.

---

![inline](https://dmitripavlutin.com/static/16d7bd44cac07b727121315ae7db1ab6/da8b6/unicode-terms.png)

---

## Unicode: Getting Comfortable
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

That means there are 16 possible characters used to represent numbers. 

* 0, 1, 2, 3, 4, 5, 6, 7, 8, and 9(base-10)
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
![inline](~/Desktop/font_subsetting_demo/cnbc_font_network_tab.png)

* 236 kb in total!!

[.build-lists: true]

---

### That's almost 3x as large as the entire Motortrend application(~86kb ish).

If CNBC serves 120,000,000 users a month(on 3G mobile, assuming no caching):

* That's *28320 Gigabytes* served over the wire
* 436ms per page load(content download) = 1.7 years collectively
* On Google Fi that would cost $283,140 to download only the fonts

We can do better.

[.build-lists: true]

---

## Font Subsetting to the rescue!
![right autoplay](https://media.giphy.com/media/J2zwN64xc4wgw/giphy.gif)

This is what we look like when we try to serve 236kb of fonts to our users

---

## What is Font Subsetting?

> Font subsets are created to provide only a minimal number of glyphs for the character set coverage, thereby reducing the redundancy of font data transmission and reducing the total number of characters transmitted.

One way to optimize performance is to serve only what is immediately needed to the user.

Font Subsetting does exactly that.

---

## Font Compression
Who can tell me why woff/woff2 aren't gzipped?

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

## Pyftsubset via FontTools

pyftsubset "myFont.woff2" \
  --output-file="myFont-basic-latin.woff2" \
  --flavor=woff2 \
  --layout-features=ccmp,locl,mark,mkmk,kern \
  --unicodes=U+0000-00A0,U+00A2-00A5,U+00A9,U+00AD-00AE,U+00B0,U+00B2-00B5,U+00B7,U+00B9-00BA,U+00D7,U+00F7,U+2000-206F,U+2074,U+20AC,U+2122,U+2190-2029,U+2031-21BB,U+2212,U+2215,U+F8FF,U+FEFF,U+FFFD

---
 
## Caveats & Legal
* Tooling is lacking in JavaScript so Python is the easiest to use currently
* [https://caniuse.com/?search=unicode-range](https://caniuse.com/?search=unicode-range)
* You need to read the font license in depth and preferably run it by legal
* Some licenses prevent the use of the Reserved Font Name, so omit the original font name if you modify it in any way
* We need a good way to maintain this, preferably with a service or config file

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
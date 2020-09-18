# Font Subsetting

## Requirements
* Python library [FontTools](https://github.com/fonttools/fonttools)
* [Wakamai Fondue](https://wakamaifondue.com/)
* [Unicode Range Spec](https://drafts.csswg.org/css-fonts-3/#unicode-range-desc)
* [pyftsubset docs](https://rsms.me/fonttools-docs/subset.html)

## Resources
* [harfbuzzjs](https://github.com/harfbuzz/harfbuzzjs)
* [glyphanger](https://github.com/filamentgroup/glyphhanger)
* [google fonts](https://developers.google.com/fonts/docs/getting_started)
* [caniuse](https://caniuse.com/?search=unicode-range)
* [What Every Dev Should Know About Unicode](https://dmitripavlutin.com/what-every-javascript-developer-should-know-about-unicode/#2-basic-unicode-terms)
* [Creating Font Subsets](https://markoskon.com/creating-font-subsets/)
* [2 stage font loading](https://www.zachleat.com/web/css-tricks-web-fonts/)
* [css tricks unicode range](https://css-tricks.com/almanac/properties/u/unicode-range/)
* [Font subsetting talk](https://www.youtube.com/watch?v=eEO77MiGOCc)

## Commands for building each font via pyftsubset(in example/)

### Basic Latin
```bash
pyftsubset "font0.woff2" \
  --output-file="Proxima-Nova-Black-Regular-basic-english.woff2" \
  --flavor=woff2 \
  --layout-features=ccmp,locl,mark,mkmk,kern \
  --unicodes=U+0000-00A0,U+00A2-00A5,U+00A9,U+00AD-00AE,U+00B0,U+00B2-00B5,U+00B7,U+00B9-00BA,U+00D7,U+00F7,U+2000-206F,U+2074,U+20AC,U+2122,U+2190-2029,U+2031-21BB,U+2212,U+2215,U+F8FF,U+FEFF,U+FFFD \
  --passthrough-tables
```

### Latin Superset
```bash
pyftsubset "font0.woff2" \
  --output-file="Proxima-Nova-Black-Regular-latin-superset.woff2" \
  --flavor=woff2 \
  --layout-features=ccmp,locl,mark,mkmk,kern \
  --unicodes=U+00A1,U+00AA-00AB,U+00AF,U+00B8,U+00BB,U+00BF-00D6,U+00D8-00F6,U+00F8-00FF,U+0131,U+0152-0153,U+02B0-02FF \
  --passthrough-tables
```

### Rest(of glyphs)
```bash
pyftsubset "font0.woff2" \
  --output-file="Proxima-Nova-Black-Regular-latin-superset.woff2" \
  --flavor=woff2 \
  --layout-features=ccmp,locl,mark,mkmk,kern \
  --unicodes=U+0100-0130,U+0132-0151,U+0154-017F,U+0180-024F,U+1E00-1EFF,U+0259,U+0300-03C0,U+2070-2073,U+2075-20AB,U+20AD-2121,U+2123-218F,U+21BC-2211,U+2213-2214,U+2216-F8FE,U+FB01-FB02 \
  --passthrough-tables
```

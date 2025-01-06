# Obsidian Sidenote Auto-Adjust Module

[中文版](README.zh-CN.md)


**One module that powers up all sidenote CSS snippets for [Obsidian.md](https://obsidian.md).**


## Features

Sidenote CSS snippets usually have hardcoded widths and margins. This `Sidenote Auto-Adjust Module.css` (SAAM) automatically calculates based on the dimension of the visible area and assigns values to the variables `--sidenote-width`, `--sidenote-margin` and `--sidenote-edgeout`, so that other sidenote snippets can be set to best suited widths.

These variables will change depending on different scenarios including:

- whether the 'Readable Line Length' option is on/off (extends the document line width to full visible area or shrinks it to 700px)
- whether the 'Show Line Numbers' option is on/off (occupies an extra space)
- whether the default theme or the Minimal theme is being used (changes the document's default line width to 40rem if the 'Readable Line Length' option is on)
- when the Minimal theme is activated, whether the current document has a Minimal-specified helper-class 'max' (changes the document's line width to 88%)
- when the Minimal theme is activated, whether the current document has a Minimal-specified helper-class 'wide' (changes the document's line width to 50rem)
- whether the current document has a Sidenote-Auto-Adjust-Module-specified helper-class 'leftsided' / 'rightsided' (shifts the document to the left/right, leaving more spaces on the right/left for sidenotes)

Depending on the available spaces in the visible area outside the document line-width, the SAAM takes one of the following approach:

- if possible, set the `--sidenote-width` to half of the document's line-width (except that when the document's line-width is 50rem, `--sidenote-width` starts with 20rem, since most people don't use a screen resolution capable of containing 50rem plus 25rem both on the left and the right)
- if the above width would put part of the sidenotes exceeding the visible area, try 75% of that base value
- if the above width still wouldn't work, try 50% (which is 1/4 of the document's line-width)
- if the available spaces are smaller than that, use the intrusive mode, in which the sidenote's outer border is `--sidenote-edgeout` outside the document's line edge, and with a suitable max-width no more than half of the document's line-width (the document's text will automatically rearrange to avoid overlapping)

## Usage

- For developpers who want to use the SAAM with your own style of sidenotes, check `Sidenote Auto-Adjust Module showcase-template.css`, which breaks down into three parts: the styling part, the positioning part and the fallback values part (which allows that snippet to work independently without SAAM)
- For common users, use `Sidenote Auto-Adjust Module.css` alongside with `Sidenote in Tufte-style using SAAM.css` and/or `Stickynote in Callout-style using SAAM.css`, which are two common styles of sidenotes that I'll explain in the following.

### Tufte-style Sidenotes

The style itself is inpired by [Tufte-CSS](https://edwardtufte.github.io/tufte-css/), but Tufte differentiates "sidenote" (that implements an auto-numbering within CSS) and "marginnote" (that doesn't have reference numbers).

This `Sidenote in Tufte-style using SAAM.css` simplifies and adopts the approach that numbering should be explicit, so that raw texts of markdown files would have better readability.

To write a sidenote, this snippet provides two syntaxes:

- Inline syntax (HTML tag with a custom "sidenote" class)
```
<span class=sidenote> text here </span>
```
- Block syntax (a hack of obsidian Callout syntax with a custom "sidenote" callout-type, inspired by [FireIsGood](https://github.com/r-u-s-h-i-k-e-s-h/Obsidian-CSS-Snippets/blob/Collection/Snippets/Sidenote%20callout%2002.md))
```
> [!sidenote]
> text here
```

Examples:

- Inline syntax
```
May the force be with you<sup>[1]</sup><span class=sidenote> $^1$ See [[Author]], _Book Name_. <br/>![](https://url/to/image.jpg) </span>
```
- Block syntax
```
> [!sidenote]
> $^1$ See [[Author]], _Book Name_.
> ![](https://url/to/image.jpg)
> 
> - list item
>
> | table cell | table cell |
> | ---------- | ---------- |
> | table cell | table cell |
(empty line)
May the force be with you<sup>[1]</sup>
```

Both syntaxes generate simplistic small-fontsize no-bakground sidenotes in both Live Preview mode and Reading mode, but the inline syntax is limited compared to the block syntax since it:

1. uses `<br/>` for line-break and cannot contain multiline markdown features like tables, lists and code blocks, and
2. renders markdown syntaxes only in Reading mode (not in Live Preview mode).

The benefit of the inline syntax is that it generates sidenotes that are precisely positioned/aligned to the referring line, whereas the block syntax is not precise and can only align with the first line of the next paragraph.

As for raw-text readability, the inline syntax is more meaningful since it relates the sidenote content with its anchor label, whereas the block syntax is simply a callout hack and is expected to be read and understood as such.

Each syntax have three variants for right, left and default:

- `<span class=sidenote-r>`,  `<span class=sidenote-l>`,  `<span class=sidenote>`
- `>[!sidenote-r]`,  `>[!sidenote-l]`,  `>[!sidenote]`

The default position can be set to left/right in the Style Settings globally across all documents, while for each document, a SAAM-specified helper-class 'leftsided' / 'rightsided' would shift that document to the left/right and set the sidenote default position to the right/left accordingly.

### Callout-style Stickynotes

This style is inspired by [sailKite](https://github.com/r-u-s-h-i-k-e-s-h/Obsidian-CSS-Snippets/blob/Collection/Snippets/Sidenote%20callout%2001.md) and [xhuajin](https://github.com/xhuajin/obsidian-sidenote-callout).

It's a hack of obsidian Callout syntax, and by design, it keeps most of the Callout's flavour, e.g. colored backgrounds and callout-titles, making it look more like a physical stickynote than just a piece of sidenote text.

To write a stickynote, this snippet provides two syntaxes:

- Simple stickynote fashion (a custom "stickynote" callout-type)
```
> [!stickynote]
> text here
```
- Full callout fashion (a custom "aside" callout-metadata)
```
> [!info|aside] (optional custom title strings and foldable markers)
> text here
```

Examples:

- Simple stickynote fashion
```
> [!stickynote]
> $^1$ See [[Author]], _Book Name_.
(empty line)
May the force be with you<sup>[1]</sup>
```
- Full callout fashion
```
> [!warning|aside]- Title: Folded note, click to unfold
> This is simply a warning
> - list item 1
> - list item 2
```

Both syntaxes render multiline markdowns in both Live Preview mode and Reading mode. The difference is that the simple fashion hides the callout-title and is more compact in line spacings, while the full fashion occupies more spaces and can use various callout-types.

Each syntax have three variants for right, left and default:

- `>[!stickynote-r]`,  `>[!stickynote-l]`,  `>[!stickynote]`
- `>[!xxxx|aside-r]`,  `>[!xxxx|aside-l]`,  `>[!xxxx|aside]`

The default position can be set to left/right in the Style Settings globally across all documents, while for each document, a SAAM-specified helper-class 'leftsided' / 'rightsided' would shift that document to the left/right and set the stickynote default position to the right/left accordingly.

## Style Settings

With the [Style Settings](https://github.com/mgmeyers/obsidian-style-settings) plugin, you can change these global settings without altering the code:

- the default position for sidenotes (left or right)
- the margin between the sidenotes and the document texts
- the intrusive-width (how much space it squeeze in the document texts) when the 'Readable Line Length' option is off (when the document fills up the visible area)
- the intrusive-width when the 'Readable Line Length' option is on and the default theme is activated
- the intrusive-width when the 'Readable Line Length' option is on and the Minimal theme is activated
- the intrusive-width when the 'Readable Line Length' option is on and the Minimal theme is activated and the document has a 'wide' helper-class
- the intrusive-width when the 'Readable Line Length' option is on and the Minimal theme is activated and the document has a 'max' helper-class
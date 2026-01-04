# Panform

Inform 7 code looks like prose, so why not write it in Markdown?

This Pandoc "writer" (plugin) turns your Markdown file into an Inform file. It might also work for other markup formats than Markdown, since Pandoc works with everything!

To test:

```
pandoc -t inform7.lua -f markdown+lists_without_preceding_blankline-smart Cloak\ of\ Darkness.ni.md | less
```


## Usage

It is assumed that you're familiar with both Inform and Markdown.

- [Install pandoc](https://pandoc.org/installing.html).
- Download `inform.lua` from this repository.
- Write an Inform story or extension in Markdown (following the syntax below), for example `Story.ni.md` or `Extension.i7x.md`.
- Transform it into Inform 7:
```
pandoc -t inform.lua Story.ni.md
```
- Since it's just Markdown, you can also transform it into HTML or whatever you want, as an alternative to `Release along with the source text.`:
```
pandoc Story.ni.md
```

This writer can be added to Obsidian's Pandoc plugin to enable you to write Inform 7 code in Obsidian and export it from there, etc.

## Syntax

First off: You can use any Inform 7 syntax you're used to in your Panform files. None of it interferes with Markdown syntax, so you can mix and match as you wish.

You can use regular Markdown syntax to format your file, so it should be properly highlighted in most Markdown editors (GitHub, Obsidian, etc).

### Quoted text

In the Inform IDE, quoted text stands out a bit (by default it's blue), which is hard to do in standard Markdown. Some Markdown varieties support highlighting text, and Panform will convert that to strings:

- \==This will be highlighted== becomes "This will be highlighted"

### Headings

Markdown headings correspond directly to [Inform headings](https://ganelson.github.io/inform-website/book/WI_2_5.html).

- \# Story/extension title and author, and extension documentation
- \#\# Volume
- \#\#\# Book
- \#\#\#\# Part
- \#\#\#\#\# Chapter
- \#\#\#\#\#\# Section

For historical reasons, those kinds of headings are called "atx-style" headings. You can of course also use the "setext-style" headings, but those only support two levels.

Few people use the full set of Inform's heading levels (Volume, Book, Part, Chapter and Section; Ryan Veeder invented the mnemonic "Very Bad People Choose Sin" to remember the hierarchy). You can customize what levels you want to use in the frontmatter:

```
---
headings:
	- two: chapter
	- three: section
---
```

This will make \#\# into Inform Chapters and \#\#\# into Sections. (You will not be allowed to redefine level one, as that's reserved for the title.)

Heading level 1 is special. For story files, it's reserved for the story title (and optionally the author), and is the only level 1 heading allowed. For extensions, it's reserved for the extension title and author, and optionally the documentation section, and those two are the only level 1 headings allowed.

- `# "Story" by Author` becomes `"Story" by Author`. The author is optional. The author can also be given in YAML frontmatter.
- `# Extension by Author` becomes `Extension by Author begins here.` (The required corresponding `Extension ends here.` is inserted automatically.) The author can also be given in YAML frontmatter.
- `# Documentation` becomes `---- DOCUMENTATION ----`. In fact, the heading title here can be anything; it's not displayed in the resulting Inform extension source code.

### Bold and emphasis

Inside strings, you can make text [bold and italic](https://ganelson.github.io/inform-website/book/RB_12_1.html) the usual ways:

- "\_italic text\_" or "\*italic text\*" becomes "[italic type]italic text[roman type]"
- "\_\_bold text\_\_" or "\*\*bold text\*\*" becomes "[bold type]bold text[roman type]"

To make fixed letter spacing, you either have to set an option in the frontmatter

### Inform 6 code

To include Inform 6 code, wrap it in backticks.

\```
! This is Inform 6 code
\```

becomes:

```
Include (-
! This is Inform 6 code
-).
```

And inline:




Sometimes, included Inform 6 code is meant to replace some existing Inform 6 code in the standard library (ie. in a kit or something). This can be done in the info string:





### Comments

Comments serve several different semantic purposes in Inform 7 (as they do in most non-literate programming languages), such as informational comments and "commented-out" code, but Inform itself only has one syntax for comments. We can be more flexible in Markdown, so all of these will become Inform 7 comments.

- Quotes
- Callouts (also called alerts or admonitions)
- Strikethrough

You can also just use regular brackets, although that won't be syntax highlighted in any Markdown parsers I know of.

### Indents

To indent code visually, use a Markdown list. Markdown does not support indents natively.

You can also choose to indent it regularly, in which case you will need to tell Pandoc that you want this.

### Tables

Markdown tables become Inform 7 tables.

### Text substitution

Use single backticks inside strings for text substition (which is what Inform 7 calls interpolation).

Regular brackets inside strings also work.

### Unicode characters and emoji

Panform can convert unicode characters and emoji to unicode codepoints.

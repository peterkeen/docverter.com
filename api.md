# API

[[_TOC_]]

## Languages

Docverter has an [official Ruby Gem](http://rubygems.org/gems/docverter) ([Github](https://github.com/docverter/docverter-ruby)). The API described here can of course be used by any language that can make HTTP requests.

## Conversions

The API has one endpoint:

    POST http://c.docverter.com/convert

The contents of your POST should be `multipart/form-data` and consist of your input file(s) and options which describe your conversion. For example:

    curl \
      --form input_files[]=@chapter1.md \
      --form input_files[]=@chapter2.md \
      --form input_files[]=@chapter3.md \
      --form from=markdown \
      --form to=pdf \
      --form css=stylesheet.css \
      --form other_files[]=@stylesheet.css \
      http://c.docverter.com/convert
      
You can also use [httpie](https://github.com/jkbr/httpie), another `curl`-like command line tool:

    http --form -f POST http://c.docverter.com/convert \
      from=markdown \
      to=html \
      input_files[]@example.md \
      --output out.html

The `examples` directory contains several examples showing off various API options.

*Note: At this time `wget` does not support `multipart/form-data` and so cannot be used to talk to Docverter*

## Full Option Reference

### General options

**`input_files[]`** *ATTACHMENT*

A single input file. This can be specified multiple times. The value should be a `multipart/form-data` file upload.

**`other_files[]`** *ATTACHMENT*

A single additional file. This can be speicifed multiple times. The value should be a `multipart/form-data` file upload.

**`from`**

Specify input format.  *FORMAT* can be `markdown` (markdown),
`textile` (Textile), `rst` (reStructuredText), `html` (HTML),
`docbook` (DocBook XML), or `latex` (LaTeX).

**`to`**

Specify output format.  *FORMAT* can be `markdown` (markdown), `rst` (reStructuredText), `html` (XHTML 1),
`latex` (LaTeX), `context` (ConTeXt), `mediawiki` (MediaWiki markup),
`textile` (Textile), `org` (Emacs Org-Mode), `texinfo` (GNU Texinfo),
`docbook` (DocBook XML), `docx` (Word docx), `epub` (EPUB book),
`mobi` (Kindle book), `asciidoc` (AsciiDoc),  or `rtf` (rich text format).


### Reader options

**`strict`**

Use strict markdown syntax, with no docverter extensions or variants.
When the input format is HTML, this means that constructs that have no
equivalents in standard markdown (e.g. definition lists or strikeout
text) will be parsed as raw HTML.

**`parse_raw`**

Parse untranslatable HTML codes and LaTeX environments as raw HTML
or LaTeX, instead of ignoring them. 

**`smart`**

Produce typographically correct output, converting straight quotes
to curly quotes, `---` to em-dashes, `--` to en-dashes, and
`...` to ellipses. Nonbreaking spaces are inserted after certain
abbreviations, such as "Mr." (Note: This option is significant only when
the input format is `markdown` or `textile`. It is selected automatically
when the input format is `textile` or the output format is `latex` or
`context`, unless `--no-tex-ligatures` is used.)

**`base_header_level`** *NUMBER*

Specify the base level for headers (defaults to 1).

**`indented_code_classes`** *CLASSES*

Specify classes to use for indented code blocks--for example,
`perl,numberLines` or `haskell`. Multiple classes may be separated by commas.

**`normalize`**

Normalize the document after reading:  merge adjacent
`Str` or `Emph` elements, for example, and remove repeated `Space`s.

**`preserve_tabs`**

Preserve tabs instead of converting them to spaces (the default).

**`tab-stop`** *NUMBER*

Specify the number of spaces per tab (default is 4).

### General writer options

**`template`** *FILE*

Use *FILE* as a custom template for the generated document. See [Templates](#templates) below for a description
of template syntax. If no extension is specified, an extension
corresponding to the writer will be added, so that `template: special`
looks for `special.html` for HTML output. If this option is not used, a default
template appropriate for the output format will be used. This file must be included using `other_files[]`.

**`no_wrap`**

Disable text wrapping in output. By default, text is wrapped
appropriately for the output format.

**`columns`** *NUMBER*

Specify length of lines in characters (for text wrapping).

**`table_of_contents`**

Include an automatically generated table of contents (or, in
the case of `latex`, `context`, and `rst`, an instruction to create
one) in the output document.

**`no_highlight`**

Disables syntax highlighting for code blocks and inlines, even when
a language attribute is given.

**`highlight_style`** *STYLE*

Specifies the coloring style to be used in highlighted source code.
Options are `pygments` (the default), `kate`, `monochrome`,
`espresso`, `zenburn`, `haddock`, and `tango`.

**`include_in_header`** *FILE*

Include contents of *FILE*, verbatim, at the end of the header.
This can be used, for example, to include special
CSS or javascript in HTML documents.  This file must be included using `other_files[]`.

**`include_before_body`** *FILE*

Include contents of *FILE*, verbatim, at the beginning of the
document body (e.g. after the `<body>` tag in HTML, or the
`\begin{document}` command in LaTeX). This can be used to include
navigation bars or banners in HTML documents.  This file must be included using `other_files[]`.

**`include_after_body`** *FILE*

Include contents of *FILE*, verbatim, at the end of the document
body (before the `</body>` tag in HTML, or the
`\end{document}` command in LaTeX).  This file must be included using `other_files[]`.
    
**`variable`** *KEY[:VAL]*

Set the template variable *KEY* to the value *VAL* when rendering the
document in standalone mode. This is generally only useful when the
`template` option is used to specify a custom template, since
docverter automatically sets the variables used in the default
templates.  If no *VAL* is specified, the key will be given the
value `true`.


### Options affecting specific writers

**`ascii`**

Use only ascii characters in output.  Currently supported only
for HTML output (which uses numerical entities instead of
UTF-8 when this option is selected).

**`reference_links`**

Use reference-style links, rather than inline links, in writing markdown
or reStructuredText.  By default inline links are used.

**`atx_headers`**

Use ATX style headers in markdown output. The default is to use
setext-style headers for levels 1-2, and then ATX headers.

**`chapters`**

Treat top-level headers as chapters in LaTeX, ConTeXt, and DocBook
output.  When the LaTeX template uses the report, book, or
memoir class, this option is implied.

**`number_sections`**

Number section headings in LaTeX, ConTeXt, or HTML output.
By default, sections are not numbered.

**`no_tex_ligatures`**

Do not convert quotation marks, apostrophes, and dashes to
the TeX ligatures when writing LaTeX or ConTeXt. Instead, just
use literal unicode characters. This is needed for using advanced
OpenType features with XeLaTeX and LuaLaTeX. Note: normally
`smart` is selected automatically for LaTeX and ConTeXt
output, but it must be specified explicitly if `no_tex_ligatures`
is selected. If you use literal curly quotes, dashes, and ellipses
in your source, then you may want to use `no_tex_ligatures`
without `smart`.

**`listings`**

Use listings package for LaTeX code blocks

**`section_divs`**

Wrap sections in `<div>` tags (or `<section>` tags in HTML5),
and attach identifiers to the enclosing `<div>` (or `<section>`)
rather than the header itself.
See [Section identifiers](#header-identifiers-in-html-latex-and-context), below.

**`email_obfuscation`** *none|javascript|references*

Specify a method for obfuscating `mailto:` links in HTML documents.
*none* leaves `mailto:` links as they are.  *javascript* obfuscates
them using javascript. *references* obfuscates them by printing their
letters as decimal or hexadecimal character references.
If `strict` is specified, *references* is used regardless of the
presence of this option.

**`id_prefix`** *STRING*

Specify a prefix to be added to all automatically generated identifiers
in HTML output.  This is useful for preventing duplicate identifiers
when generating fragments to be included in other pages.

**`title_prefix`** *STRING*

Specify *STRING* as a prefix at the beginning of the title
that appears in the HTML header (but not in the title as it
appears at the beginning of the HTML body).

**`css=`** *URL*

Link to a CSS style sheet.

**`reference_docx`** *FILE*

Use the specified file as a style reference in producing a docx file.
For best results, the reference docx should be a modified version
of a docx file produced using docverter.  The contents of the reference docx
are ignored, but its stylesheets are used in the new docx.  This file must be included using `other_files[]`.

**`pdf_username`** *STRING*

Encrypt the output PDF with the given username.

**`pdf_password`** *STRING*

Encrypt the output PDF with the given password.

**`epub_stylesheet`** *FILE*

Use the specified CSS file to style the EPUB.  This file must be included using `other_files[]`.

**`epub_cover_image`** *FILE*

Use the specified image as the EPUB cover.  It is recommended
that the image be less than 1000px in width and height.  This file must be included using `other_files[]`.

**`epub_metadata`** *FILE*

Look in the specified XML file for metadata for the EPUB.
The file should contain a series of Dublin Core elements,
as documented at <http://dublincore.org/documents/dces/>.
For example:

     <dc:rights>Creative Commons</dc:rights>
     <dc:language>es-AR</dc:language>

By default, docverter will include the following metadata elements:
`<dc:title>` (from the document title), `<dc:creator>` (from the
document authors), `<dc:date>` (from the document date, which should
be in [ISO 8601 format]), `<dc:language>` (from the `lang`
variable, or, if is not set, the locale), and `<dc:identifier
id="BookId">` (a randomly generated UUID). Any of these may be
overridden by elements in the metadata file.  This file must be included using `other_files[]`.

**`epub_embed_font`** *FILE*

Embed the specified font in the EPUB. This option can be an
array to embed multiple fonts.  To use embedded fonts, you
will need to add declarations like the following to your CSS (see
`epub_stylesheet`):

    @font-face {
    font-family: DejaVuSans;
    font-style: normal;
    font-weight: normal;
    src:url("DejaVuSans-Regular.ttf");
    }
    @font-face {
    font-family: DejaVuSans;
    font-style: normal;
    font-weight: bold;
    src:url("DejaVuSans-Bold.ttf");
    }
    @font-face {
    font-family: DejaVuSans;
    font-style: italic;
    font-weight: normal;
    src:url("DejaVuSans-Oblique.ttf");
    }
    @font-face {
    font-family: DejaVuSans;
    font-style: italic;
    font-weight: bold;
    src:url("DejaVuSans-BoldOblique.ttf");
    }
    body { font-family: "DejaVuSans"; }

This file must be included using `other_files[]`.


## Templates

Docverter  uses a template to
add header and footer material that is needed for a self-standing
document.  A custom template
can be specified using the `template` option.

Templates may contain *variables*.  Variable names are sequences of
alphanumerics, `-`, and `_`, starting with a letter.  A variable name
surrounded by `$` signs will be replaced by its value.  For example,
the string `$title$` in

    <title>$title$</title>

will be replaced by the document title.

To write a literal `$` in a template, use `$$`.

Some variables are set automatically by docverter.  These vary somewhat
depending on the output format, but include:

**`header-includes`**

contents specified by `include_in_header` (may have multiple
values)

**`toc`**

non-null value if `table_of_contents` was specified

**`include-before`**

contents specified by `include_before_body` (may have
multiple values)

**`include-after`**

contents specified by `include_after_body` (may have
multiple values)

**`body`**

body of document

**`title`**

title of document, as specified in title block

**`author`**

author of document, as specified in title block (may have
multiple values)

**`date`**

date of document, as specified in title block

**`lang`**

language code for HTML or LaTeX documents

**`fontsize`**

font size (10pt, 11pt, 12pt) for LaTeX documents

**`documentclass`**

document class for LaTeX documents

**`geometry`**

options for LaTeX `geometry` class, e.g. `margin=1in`;
may be repeated for multiple options

**`mainfont`**, **`sansfont`**, **`monofont`**, **`mathfont`**

fonts for LaTeX documents (works only with xelatex
and lualatex)

**`linkcolor`**

color for internal links in LaTeX documents (`red`, `green`,
`magenta`, `cyan`, `blue`, `black`)

**`urlcolor`**

color for external links in LaTeX documents

**`links-as-notes`**

causes links to be printed as footnotes in LaTeX documents

Variables may be set in the manifest using the `variable`
option. This allows users to include custom variables in their
templates.

Templates may contain conditionals.  The syntax is as follows:

    $if(variable)$
    X
    $else$
    Y
    $endif$

This will include `X` in the template if `variable` has a non-null
value; otherwise it will include `Y`. `X` and `Y` are placeholders for
any valid template text, and may include interpolated variables or other
conditionals. The `$else$` section may be omitted.

When variables can have multiple values (for example, `author` in
a multi-author document), you can use the `$for$` keyword:

    $for(author)$
    <meta name="author" content="$author$" />
    $endfor$

You can optionally specify a separator to be used between
consecutive items:

    $for(author)$$author$$sep$, $endfor$

If you use custom templates, you may need to revise them as pandoc
changes.  We recommend tracking the changes in the default templates,
and modifying your custom templates accordingly. An easy way to do this
is to fork the pandoc-templates repository
(<http://github.com/jgm/pandoc-templates>) and merge in changes after each
pandoc release.

## Docverter's markdown

Docverter understands an extended and slightly revised version of
John Gruber's [markdown][] syntax.  This document explains the syntax,
noting differences from standard markdown. Except where noted, these
differences can be suppressed by specifying the `--strict` command-line
option.

### Philosophy

Markdown is designed to be easy to write, and, even more importantly,
easy to read:

> A Markdown-formatted document should be publishable as-is, as plain
> text, without looking like it's been marked up with tags or formatting
> instructions.
> -- [John Gruber](http://daringfireball.net/projects/markdown/syntax#philosophy)

This principle has guided docverter's decisions in finding syntax for
tables, footnotes, and other extensions.

There is, however, one respect in which docverter's aims are different
from the original aims of markdown.  Whereas markdown was originally
designed with HTML generation in mind, docverter is designed for multiple
output formats.  Thus, while docverter allows the embedding of raw HTML,
it discourages it, and provides other, non-HTMLish ways of representing
important document elements like definition lists, tables, mathematics, and
footnotes.

### Paragraphs

A paragraph is one or more lines of text followed by one or more blank line.
Newlines are treated as spaces, so you can reflow your paragraphs as you like.
If you need a hard line break, put two or more spaces at the end of a line,
or type a backslash followed by a newline.

### Headers

There are two kinds of headers, Setext and atx.

#### Setext-style headers ###

A setext-style header is a line of text "underlined" with a row of `=` signs
(for a level one header) of `-` signs (for a level two header):

    A level-one header
    ==================

    A level-two header
    ------------------

The header text can contain inline formatting, such as emphasis (see
[Inline formatting](#inline-formatting), below).


#### Atx-style headers ###

An Atx-style header consists of one to six `#` signs and a line of
text, optionally followed by any number of `#` signs.  The number of
`#` signs at the beginning of the line is the header level:

    ## A level-two header

    ### A level-three header ###

As with setext-style headers, the header text can contain formatting:

    # A level-one header with a [link](/url) and *emphasis*

Standard markdown syntax does not require a blank line before a header.
Docverter does require this (except, of course, at the beginning of the
document). The reason for the requirement is that it is all too easy for a
`#` to end up at the beginning of a line by accident (perhaps through line
wrapping). Consider, for example:

    I like several of their flavors of ice cream:
    #22, for example, and #5.


#### Header identifiers in HTML, LaTeX, and ConTeXt ###

*Docverter extension*.

Each header element in docverter's HTML and ConTeXt output is given a
unique identifier. This identifier is based on the text of the header.
To derive the identifier from the header text,

  - Remove all formatting, links, etc.
  - Remove all punctuation, except underscores, hyphens, and periods.
  - Replace all spaces and newlines with hyphens.
  - Convert all alphabetic characters to lowercase.
  - Remove everything up to the first letter (identifiers may
    not begin with a number or punctuation mark).
  - If nothing is left after this, use the identifier `section`.

Thus, for example,

  Header                            Identifier
  -------------------------------   ----------------------------
  Header identifiers in HTML        `header-identifiers-in-html`
  *Dogs*?--in *my* house?           `dogs--in-my-house`
  [HTML], [S5], or [RTF]?           `html-s5-or-rtf`
  3. Applications                   `applications`
  33                                `section`

These rules should, in most cases, allow one to determine the identifier
from the header text. The exception is when several headers have the
same text; in this case, the first will get an identifier as described
above; the second will get the same identifier with `-1` appended; the
third with `-2`; and so on.

These identifiers are used to provide link targets in the table of
contents generated by the `--toc|--table-of-contents` option. They
also make it easy to provide links from one section of a document to
another. A link to this section, for example, might look like this:

    See the section on
    [header identifiers](#header-identifiers-in-html).

Note, however, that this method of providing links to sections works
only in HTML, LaTeX, and ConTeXt formats.

If the `--section-divs` option is specified, then each section will
be wrapped in a `div` (or a `section`, if `--html5` was specified),
and the identifier will be attached to the enclosing `<div>`
(or `<section>`) tag rather than the header itself. This allows entire
sections to be manipulated using javascript or treated differently in
CSS.

### Block quotations

Markdown uses email conventions for quoting blocks of text.
A block quotation is one or more paragraphs or other block elements
(such as lists or headers), with each line preceded by a `>` character
and a space. (The `>` need not start at the left margin, but it should
not be indented more than three spaces.)

    > This is a block quote. This
    > paragraph has two lines.
    >
    > 1. This is a list inside a block quote.
    > 2. Second item.

A "lazy" form, which requires the `>` character only on the first
line of each block, is also allowed:

    > This is a block quote. This
    paragraph has two lines.

    > 1. This is a list inside a block quote.
    2. Second item.

Among the block elements that can be contained in a block quote are
other block quotes. That is, block quotes can be nested:

    > This is a block quote.
    >
    > > A block quote within a block quote.

Standard markdown syntax does not require a blank line before a block
quote.  Docverter does require this (except, of course, at the beginning of the
document). The reason for the requirement is that it is all too easy for a
`>` to end up at the beginning of a line by accident (perhaps through line
wrapping). So, unless `--strict` is used, the following does not produce
a nested block quote in docverter:

    > This is a block quote.
    >> Nested.


### Verbatim (code) blocks

#### Indented code blocks ###

A block of text indented four spaces (or one tab) is treated as verbatim
text: that is, special characters do not trigger special formatting,
and all spaces and line breaks are preserved.  For example,

        if (a > 3) {
          moveShip(5 * gravity, DOWN);
        }

The initial (four space or one tab) indentation is not considered part
of the verbatim text, and is removed in the output.

Note: blank lines in the verbatim text need not begin with four spaces.

#### Delimited code blocks ###

*Docverter extension*.

In addition to standard indented code blocks, Docverter supports
*delimited* code blocks.  These begin with a row of three or more
tildes (`~`) or backticks (`` ` ``) and end with a row of tildes or
backticks that must be at least as long as the starting row. Everything
between these lines is treated as code. No indentation is necessary:

    ~~~~~~~
    if (a > 3) {
      moveShip(5 * gravity, DOWN);
    }
    ~~~~~~~

Like regular code blocks, delimited code blocks must be separated
from surrounding text by blank lines.

If the code itself contains a row of tildes or backticks, just use a longer
row of tildes or backticks at the start and end:

    ~~~~~~~~~~~~~~~~
    ~~~~~~~~~~
    code including tildes
    ~~~~~~~~~~
    ~~~~~~~~~~~~~~~~

Optionally, you may attach attributes to the code block using
this syntax:

    ~~~~ {#mycode .haskell .numberLines startFrom="100"}
    qsort []     = []
    qsort (x:xs) = qsort (filter (< x) xs) ++ [x] ++
                   qsort (filter (>= x) xs)
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


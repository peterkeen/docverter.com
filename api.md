# API

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

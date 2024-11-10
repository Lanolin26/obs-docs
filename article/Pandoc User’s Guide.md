---
source: https://pandoc.org/MANUAL.html
---
[Please help Ukraine!](https://novaukraine.org)

[Sponsor](https://github.com/users/jgm/sponsorship)

<img src="https://www.paypalobjects.com/en_US/i/scr/pixel.gif" width="1" height="1" />

<span class="big">Pandoc</span>   <span class="small">a universal document converter</span>

-   <a href="index.html" class="nav-link">About</a>
-   <a href="installing.html" class="nav-link">Installing</a>
-   <a href="demos.html" class="nav-link">Demos</a>
-   <a href="#" id="navbarDropdown" class="nav-link dropdown-toggle">Documentation</a>
    -   <a href="getting-started.html" class="dropdown-item">Getting started</a>
    -   <a href="MANUAL.html" class="dropdown-item">User's Guide</a>
    -   <a href="MANUAL.pdf" class="dropdown-item">User's Guide (PDF)</a>
    -   <a href="CONTRIBUTING.html" class="dropdown-item">Contributing</a>
    -   <a href="faqs.html" class="dropdown-item">FAQ</a>
    -   <a href="press.html" class="dropdown-item">Press</a>
    -   <a href="filters.html" class="dropdown-item">Filters</a>
    -   <a href="lua-filters.html" class="dropdown-item">Lua filters</a>
    -   <a href="custom-readers.html" class="dropdown-item">Custom readers</a>
    -   <a href="custom-writers.html" class="dropdown-item">Custom writers</a>
    -   <a href="pandoc-server.html" class="dropdown-item">pandoc-server</a>
    -   <a href="epub.html" class="dropdown-item">Making an ebook</a>
    -   <a href="org.html" class="dropdown-item">Emacs Org mode support</a>
    -   <a href="jats.html" class="dropdown-item">JATS support</a>
    -   <a href="typst-property-output.html" class="dropdown-item">Typst property attributes</a>
    -   <a href="using-the-pandoc-api.html" class="dropdown-item">Using the Pandoc API</a>
    -   <a href="http://hackage.haskell.org/package/pandoc" class="dropdown-item">API documentation</a>
-   <a href="help.html" class="nav-link">Help</a>
-   <a href="extras.html" class="nav-link">Extras</a>
-   <a href="releases.html" class="nav-link">Releases</a>
-   Search

<a href="#synopsis" class="anchor"></a>Synopsis <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
===============================================================================================================

<span class="nowrap">`pandoc`</span> \[*options*\] \[*input-file*\]…

<a href="#description" class="anchor"></a>Description <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=====================================================================================================================

Pandoc is a [Haskell](https://www.haskell.org) library for converting from one markup format to another, and a command-line tool that uses this library.

Pandoc can convert between numerous markup and word processing formats, including, but not limited to, various flavors of [Markdown](https://daringfireball.net/projects/markdown/), [HTML](https://www.w3.org/html/), [LaTeX](https://www.latex-project.org/) and [Word docx](https://en.wikipedia.org/wiki/Office_Open_XML). For the full lists of input and output formats, see the <a href="#option--from" class="option"><span class="nowrap"><code>--from</code></span></a> and <a href="#option--to" class="option"><span class="nowrap"><code>--to</code></span></a> [options below](#general-options). Pandoc can also produce [PDF](https://www.adobe.com/pdf/) output: see [creating a PDF](#creating-a-pdf), below.

Pandoc’s enhanced version of Markdown includes syntax for [tables](#tables), [definition lists](#definition-lists), [metadata blocks](#metadata-blocks), [footnotes](#footnotes), [citations](#citations), [math](#math), and much more. See below under [Pandoc’s Markdown](#pandocs-markdown).

Pandoc has a modular design: it consists of a set of readers, which parse text in a given format and produce a native representation of the document (an *abstract syntax tree* or AST), and a set of writers, which convert this native representation into a target format. Thus, adding an input or output format requires only adding a reader or writer. Users can also run custom [pandoc filters](https://pandoc.org/filters.html) to modify the intermediate AST.

Because pandoc’s intermediate representation of a document is less expressive than many of the formats it converts between, one should not expect perfect conversions between every format and every other. Pandoc attempts to preserve the structural elements of a document, but not formatting details such as margin size. And some document elements, such as complex tables, may not fit into pandoc’s simple document model. While conversions from pandoc’s Markdown to all formats aspire to be perfect, conversions from formats more expressive than pandoc’s Markdown can be expected to be lossy.

<a href="#using-pandoc" class="anchor"></a>Using pandoc <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------

If no *input-files* are specified, input is read from *stdin*. Output goes to *stdout* by default. For output to a file, use the <a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a> option:

    pandoc -o output.html input.txt

By default, pandoc produces a document fragment. To produce a standalone document (e.g. a valid HTML file including <span class="nowrap">`<head>`</span> and <span class="nowrap">`<body>`</span>), use the <a href="#option--standalone" class="option"><span class="nowrap"><code>-s</code></span></a> or <a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a> flag:

    pandoc -s -o output.html input.txt

For more information on how standalone documents are produced, see [Templates](#templates) below.

If multiple input files are given, pandoc will concatenate them all (with blank lines between them) before parsing. (Use <a href="#option--file-scope%5B" class="option"><span class="nowrap"><code>--file-scope</code></span></a> to parse files individually.)

<a href="#specifying-formats" class="anchor"></a>Specifying formats <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------------

The format of the input and output can be specified explicitly using command-line options. The input format can be specified using the <a href="#option--from" class="option"><span class="nowrap"><code>-f/--from</code></span></a> option, the output format using the <a href="#option--to" class="option"><span class="nowrap"><code>-t/--to</code></span></a> option. Thus, to convert <span class="nowrap">`hello.txt`</span> from Markdown to LaTeX, you could type:

    pandoc -f markdown -t latex hello.txt

To convert <span class="nowrap">`hello.html`</span> from HTML to Markdown:

    pandoc -f html -t markdown hello.html

Supported input and output formats are listed below under [Options](#options) (see <a href="#option--from" class="option"><span class="nowrap"><code>-f</code></span></a> for input formats and <a href="#option--to" class="option"><span class="nowrap"><code>-t</code></span></a> for output formats). You can also use <span class="nowrap">`pandoc --list-input-formats`</span> and <span class="nowrap">`pandoc --list-output-formats`</span> to print lists of supported formats.

If the input or output format is not specified explicitly, pandoc will attempt to guess it from the extensions of the filenames. Thus, for example,

    pandoc -o hello.tex hello.txt

will convert <span class="nowrap">`hello.txt`</span> from Markdown to LaTeX. If no output file is specified (so that output goes to *stdout*), or if the output file’s extension is unknown, the output format will default to HTML. If no input file is specified (so that input comes from *stdin*), or if the input files’ extensions are unknown, the input format will be assumed to be Markdown.

<a href="#character-encoding" class="anchor"></a>Character encoding <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------------

Pandoc uses the UTF-8 character encoding for both input and output. If your local character encoding is not UTF-8, you should pipe input and output through [<span class="nowrap">`iconv`</span>](https://www.gnu.org/software/libiconv/):

    iconv -t utf-8 input.txt | pandoc | iconv -f utf-8

Note that in some output formats (such as HTML, LaTeX, ConTeXt, RTF, OPML, DocBook, and Texinfo), information about the character encoding is included in the document header, which will only be included if you use the <a href="#option--standalone" class="option"><span class="nowrap"><code>-s/--standalone</code></span></a> option.

<a href="#creating-a-pdf" class="anchor"></a>Creating a PDF <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------

To produce a PDF, specify an output file with a <span class="nowrap">`.pdf`</span> extension:

    pandoc test.txt -o test.pdf

By default, pandoc will use LaTeX to create the PDF, which requires that a LaTeX engine be installed (see <a href="#option--pdf-engine" class="option"><span class="nowrap"><code>--pdf-engine</code></span></a> below). Alternatively, pandoc can use ConTeXt, roff ms, or HTML as an intermediate format. To do this, specify an output file with a <span class="nowrap">`.pdf`</span> extension, as before, but add the <a href="#option--pdf-engine" class="option"><span class="nowrap"><code>--pdf-engine</code></span></a> option or <a href="#option--to" class="option"><span class="nowrap"><code>-t context</code></span></a>, <a href="#option--to" class="option"><span class="nowrap"><code>-t html</code></span></a>, or <a href="#option--to" class="option"><span class="nowrap"><code>-t ms</code></span></a> to the command line. The tool used to generate the PDF from the intermediate format may be specified using <a href="#option--pdf-engine" class="option"><span class="nowrap"><code>--pdf-engine</code></span></a>.

You can control the PDF style using variables, depending on the intermediate format used: see [variables for LaTeX](#variables-for-latex), [variables for ConTeXt](#variables-for-context), [variables for <span class="nowrap">`wkhtmltopdf`</span>](#variables-for-wkhtmltopdf), [variables for ms](#variables-for-ms). When HTML is used as an intermediate format, the output can be styled using <a href="#option--css" class="option"><span class="nowrap"><code>--css</code></span></a>.

To debug the PDF creation, it can be useful to look at the intermediate representation: instead of <a href="#option--output" class="option"><span class="nowrap"><code>-o test.pdf</code></span></a>, use for example <a href="#option--standalone" class="option"><span class="nowrap"><code>-s -o test.tex</code></span></a> to output the generated LaTeX. You can then test it with <span class="nowrap">`pdflatex test.tex`</span>.

When using LaTeX, the following packages need to be available (they are included with all recent versions of [TeX Live](https://www.tug.org/texlive/)): [<span class="nowrap">`amsfonts`</span>](https://ctan.org/pkg/amsfonts), [<span class="nowrap">`amsmath`</span>](https://ctan.org/pkg/amsmath), [<span class="nowrap">`lm`</span>](https://ctan.org/pkg/lm), [<span class="nowrap">`unicode-math`</span>](https://ctan.org/pkg/unicode-math), [<span class="nowrap">`iftex`</span>](https://ctan.org/pkg/iftex), [<span class="nowrap">`listings`</span>](https://ctan.org/pkg/listings) (if the <a href="#option--listings%5B" class="option"><span class="nowrap"><code>--listings</code></span></a> option is used), [<span class="nowrap">`fancyvrb`</span>](https://ctan.org/pkg/fancyvrb), [<span class="nowrap">`longtable`</span>](https://ctan.org/pkg/longtable), [<span class="nowrap">`booktabs`</span>](https://ctan.org/pkg/booktabs), \[<span class="nowrap">`multirow`</span>\] (if the document contains a table with cells that cross multiple rows), [<span class="nowrap">`graphicx`</span>](https://ctan.org/pkg/graphicx) (if the document contains images), [<span class="nowrap">`bookmark`</span>](https://ctan.org/pkg/bookmark), [<span class="nowrap">`xcolor`</span>](https://ctan.org/pkg/xcolor), [<span class="nowrap">`soul`</span>](https://ctan.org/pkg/soul), [<span class="nowrap">`geometry`</span>](https://ctan.org/pkg/geometry) (with the <span class="nowrap">`geometry`</span> variable set), [<span class="nowrap">`setspace`</span>](https://ctan.org/pkg/setspace) (with <span class="nowrap">`linestretch`</span>), and [<span class="nowrap">`babel`</span>](https://ctan.org/pkg/babel) (with <span class="nowrap">`lang`</span>). If <span class="nowrap">`CJKmainfont`</span> is set, [<span class="nowrap">`xeCJK`</span>](https://ctan.org/pkg/xecjk) is needed if <span class="nowrap">`xelatex`</span> is used, else [<span class="nowrap">`luatexja`</span>](https://ctan.org/pkg/luatexja) is needed if <span class="nowrap">`lualatex`</span> is used. [<span class="nowrap">`framed`</span>](https://ctan.org/pkg/framed) is required if code is highlighted in a scheme that use a colored background. The use of <span class="nowrap">`xelatex`</span> or <span class="nowrap">`lualatex`</span> as the PDF engine requires [<span class="nowrap">`fontspec`</span>](https://ctan.org/pkg/fontspec). <span class="nowrap">`lualatex`</span> uses [<span class="nowrap">`selnolig`</span>](https://ctan.org/pkg/selnolig) and [<span class="nowrap">`lua-ul`</span>](https://ctan.org/pkg/lua-ul). <span class="nowrap">`xelatex`</span> uses [<span class="nowrap">`bidi`</span>](https://ctan.org/pkg/bidi) (with the <span class="nowrap">`dir`</span> variable set). If the <span class="nowrap">`mathspec`</span> variable is set, <span class="nowrap">`xelatex`</span> will use [<span class="nowrap">`mathspec`</span>](https://ctan.org/pkg/mathspec) instead of [<span class="nowrap">`unicode-math`</span>](https://ctan.org/pkg/unicode-math). The [<span class="nowrap">`upquote`</span>](https://ctan.org/pkg/upquote) and [<span class="nowrap">`microtype`</span>](https://ctan.org/pkg/microtype) packages are used if available, and [<span class="nowrap">`csquotes`</span>](https://ctan.org/pkg/csquotes) will be used for [typography](#typography) if the <span class="nowrap">`csquotes`</span> variable or metadata field is set to a true value. The [<span class="nowrap">`natbib`</span>](https://ctan.org/pkg/natbib), [<span class="nowrap">`biblatex`</span>](https://ctan.org/pkg/biblatex), [<span class="nowrap">`bibtex`</span>](https://ctan.org/pkg/bibtex), and [<span class="nowrap">`biber`</span>](https://ctan.org/pkg/biber) packages can optionally be used for [citation rendering](#citation-rendering). The following packages will be used to improve output quality if present, but pandoc does not require them to be present: [<span class="nowrap">`upquote`</span>](https://ctan.org/pkg/upquote) (for straight quotes in verbatim environments), [<span class="nowrap">`microtype`</span>](https://ctan.org/pkg/microtype) (for better spacing adjustments), [<span class="nowrap">`parskip`</span>](https://ctan.org/pkg/parskip) (for better inter-paragraph spaces), [<span class="nowrap">`xurl`</span>](https://ctan.org/pkg/xurl) (for better line breaks in URLs), and [<span class="nowrap">`footnotehyper`</span>](https://ctan.org/pkg/footnotehyper) or [<span class="nowrap">`footnote`</span>](https://ctan.org/pkg/footnote) (to allow footnotes in tables).

<a href="#reading-from-the-web" class="anchor"></a>Reading from the Web <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------------------

Instead of an input file, an absolute URI may be given. In this case pandoc will fetch the content using HTTP:

    pandoc -f html -t markdown https://www.fsf.org

It is possible to supply a custom User-Agent string or other header when requesting a document from a URL:

    pandoc -f html -t markdown --request-header User-Agent:"Mozilla/5.0" \
      https://www.fsf.org

<a href="#options" class="anchor"></a>Options <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=============================================================================================================

<a href="#general-options" class="anchor"></a>General options <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------

<span id="option--from" class="option-anchor"><span class="nowrap">`-f`</span> *FORMAT*, <span class="nowrap">`-r`</span> *FORMAT*, <span class="nowrap">`--from=`</span>*FORMAT*, <span class="nowrap">`--read=`</span>*FORMAT*</span>  
Specify input format. *FORMAT* can be:

-   <span class="nowrap">`bibtex`</span> ([BibTeX](https://ctan.org/pkg/bibtex) bibliography)
-   <span class="nowrap">`biblatex`</span> ([BibLaTeX](https://ctan.org/pkg/biblatex) bibliography)
-   <span class="nowrap">`bits`</span> ([BITS](https://jats.nlm.nih.gov/extensions/bits/) XML, alias for <span class="nowrap">`jats`</span>)
-   <span class="nowrap">`commonmark`</span> ([CommonMark](https://commonmark.org) Markdown)
-   <span class="nowrap">`commonmark_x`</span> ([CommonMark](https://commonmark.org) Markdown with extensions)
-   <span class="nowrap">`creole`</span> ([Creole 1.0](http://www.wikicreole.org/wiki/Creole1.0))
-   <span class="nowrap">`csljson`</span> ([CSL JSON](https://citeproc-js.readthedocs.io/en/latest/csl-json/markup.html) bibliography)
-   <span class="nowrap">`csv`</span> ([CSV](https://tools.ietf.org/html/rfc4180) table)
-   <span class="nowrap">`tsv`</span> ([TSV](https://www.iana.org/assignments/media-types/text/tab-separated-values) table)
-   <span class="nowrap">`djot`</span> ([Djot markup](https://djot.net))
-   <span class="nowrap">`docbook`</span> ([DocBook](https://docbook.org))
-   <span class="nowrap">`docx`</span> ([Word docx](https://en.wikipedia.org/wiki/Office_Open_XML))
-   <span class="nowrap">`dokuwiki`</span> ([DokuWiki markup](https://www.dokuwiki.org/dokuwiki))
-   <span class="nowrap">`endnotexml`</span> ([EndNote XML bibliography](https://support.clarivate.com/Endnote/s/article/EndNote-XML-Document-Type-Definition))
-   <span class="nowrap">`epub`</span> ([EPUB](http://idpf.org/epub))
-   <span class="nowrap">`fb2`</span> ([FictionBook2](http://www.fictionbook.org/index.php/Eng:XML_Schema_Fictionbook_2.1) e-book)
-   <span class="nowrap">`gfm`</span> ([GitHub-Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/)), or the deprecated and less accurate <span class="nowrap">`markdown_github`</span>; use [<span class="nowrap">`markdown_github`</span>](#markdown-variants) only if you need extensions not supported in [<span class="nowrap">`gfm`</span>](#markdown-variants).
-   <span class="nowrap">`haddock`</span> ([Haddock markup](https://www.haskell.org/haddock/doc/html/ch03s08.html))
-   <span class="nowrap">`html`</span> ([HTML](https://www.w3.org/html/))
-   <span class="nowrap">`ipynb`</span> ([Jupyter notebook](https://nbformat.readthedocs.io/en/latest/))
-   <span class="nowrap">`jats`</span> ([JATS](https://jats.nlm.nih.gov) XML)
-   <span class="nowrap">`jira`</span> ([Jira](https://jira.atlassian.com/secure/WikiRendererHelpAction.jspa?section=all)/Confluence wiki markup)
-   <span class="nowrap">`json`</span> (JSON version of native AST)
-   <span class="nowrap">`latex`</span> ([LaTeX](https://www.latex-project.org/))
-   <span class="nowrap">`markdown`</span> ([Pandoc’s Markdown](#pandocs-markdown))
-   <span class="nowrap">`markdown_mmd`</span> ([MultiMarkdown](https://fletcherpenney.net/multimarkdown/))
-   <span class="nowrap">`markdown_phpextra`</span> ([PHP Markdown Extra](https://michelf.ca/projects/php-markdown/extra/))
-   <span class="nowrap">`markdown_strict`</span> (original unextended [Markdown](https://daringfireball.net/projects/markdown/))
-   <span class="nowrap">`mediawiki`</span> ([MediaWiki markup](https://www.mediawiki.org/wiki/Help:Formatting))
-   <span class="nowrap">`man`</span> ([roff man](https://man.cx/groff_man(7)))
-   <span class="nowrap">`muse`</span> ([Muse](https://amusewiki.org/library/manual))
-   <span class="nowrap">`native`</span> (native Haskell)
-   <span class="nowrap">`odt`</span> ([OpenOffice text document](https://en.wikipedia.org/wiki/OpenDocument))
-   <span class="nowrap">`opml`</span> ([OPML](http://dev.opml.org/spec2.html))
-   <span class="nowrap">`org`</span> ([Emacs Org mode](https://orgmode.org))
-   <span class="nowrap">`ris`</span> ([RIS](https://en.wikipedia.org/wiki/RIS_(file_format)) bibliography)
-   <span class="nowrap">`rtf`</span> ([Rich Text Format](https://en.wikipedia.org/wiki/Rich_Text_Format))
-   <span class="nowrap">`rst`</span> ([reStructuredText](https://docutils.sourceforge.io/docs/ref/rst/introduction.html))
-   <span class="nowrap">`t2t`</span> ([txt2tags](https://txt2tags.org))
-   <span class="nowrap">`textile`</span> ([Textile](https://textile-lang.com))
-   <span class="nowrap">`tikiwiki`</span> ([TikiWiki markup](https://doc.tiki.org/Wiki-Syntax-Text#The_Markup_Language_Wiki-Syntax))
-   <span class="nowrap">`twiki`</span> ([TWiki markup](https://twiki.org/cgi-bin/view/TWiki/TextFormattingRules))
-   <span class="nowrap">`typst`</span> ([typst](https://typst.app))
-   <span class="nowrap">`vimwiki`</span> ([Vimwiki](https://vimwiki.github.io))
-   the path of a custom Lua reader, see [Custom readers and writers](#custom-readers-and-writers) below

Extensions can be individually enabled or disabled by appending <span class="nowrap">`+EXTENSION`</span> or <span class="nowrap">`-EXTENSION`</span> to the format name. See [Extensions](#extensions) below, for a list of extensions and their names. See <a href="#option--list-input-formats" class="option"><span class="nowrap"><code>--list-input-formats</code></span></a> and <a href="#option--list-extensions" class="option"><span class="nowrap"><code>--list-extensions</code></span></a>, below.

<span id="option--to" class="option-anchor"><span class="nowrap">`-t`</span> *FORMAT*, <span class="nowrap">`-w`</span> *FORMAT*, <span class="nowrap">`--to=`</span>*FORMAT*, <span class="nowrap">`--write=`</span>*FORMAT*</span>  
Specify output format. *FORMAT* can be:

-   <span class="nowrap">`ansi`</span> (text with [ANSI escape codes](https://en.wikipedia.org/wiki/ANSI_escape_code), for terminal viewing)
-   <span class="nowrap">`asciidoc`</span> (modern [AsciiDoc](https://asciidoc.org/) as interpreted by [AsciiDoctor](https://asciidoctor.org/))
-   <span class="nowrap">`asciidoc_legacy`</span> ([AsciiDoc](https://asciidoc.org/) as interpreted by [<span class="nowrap">`asciidoc-py`</span>](https://github.com/asciidoc-py/asciidoc-py)).
-   <span class="nowrap">`asciidoctor`</span> (deprecated synonym for <span class="nowrap">`asciidoc`</span>)
-   <span class="nowrap">`beamer`</span> ([LaTeX beamer](https://ctan.org/pkg/beamer) slide show)
-   <span class="nowrap">`bibtex`</span> ([BibTeX](https://ctan.org/pkg/bibtex) bibliography)
-   <span class="nowrap">`biblatex`</span> ([BibLaTeX](https://ctan.org/pkg/biblatex) bibliography)
-   <span class="nowrap">`chunkedhtml`</span> (zip archive of multiple linked HTML files)
-   <span class="nowrap">`commonmark`</span> ([CommonMark](https://commonmark.org) Markdown)
-   <span class="nowrap">`commonmark_x`</span> ([CommonMark](https://commonmark.org) Markdown with extensions)
-   <span class="nowrap">`context`</span> ([ConTeXt](https://www.contextgarden.net/))
-   <span class="nowrap">`csljson`</span> ([CSL JSON](https://citeproc-js.readthedocs.io/en/latest/csl-json/markup.html) bibliography)
-   <span class="nowrap">`djot`</span> ([Djot markup](https://djot.net))
-   <span class="nowrap">`docbook`</span> or <span class="nowrap">`docbook4`</span> ([DocBook](https://docbook.org) 4)
-   <span class="nowrap">`docbook5`</span> (DocBook 5)
-   <span class="nowrap">`docx`</span> ([Word docx](https://en.wikipedia.org/wiki/Office_Open_XML))
-   <span class="nowrap">`dokuwiki`</span> ([DokuWiki markup](https://www.dokuwiki.org/dokuwiki))
-   <span class="nowrap">`epub`</span> or <span class="nowrap">`epub3`</span> ([EPUB](http://idpf.org/epub) v3 book)
-   <span class="nowrap">`epub2`</span> (EPUB v2)
-   <span class="nowrap">`fb2`</span> ([FictionBook2](http://www.fictionbook.org/index.php/Eng:XML_Schema_Fictionbook_2.1) e-book)
-   <span class="nowrap">`gfm`</span> ([GitHub-Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/)), or the deprecated and less accurate <span class="nowrap">`markdown_github`</span>; use [<span class="nowrap">`markdown_github`</span>](#markdown-variants) only if you need extensions not supported in [<span class="nowrap">`gfm`</span>](#markdown-variants).
-   <span class="nowrap">`haddock`</span> ([Haddock markup](https://www.haskell.org/haddock/doc/html/ch03s08.html))
-   <span class="nowrap">`html`</span> or <span class="nowrap">`html5`</span> ([HTML](https://www.w3.org/html/), i.e. [HTML5](https://html.spec.whatwg.org/)/XHTML [polyglot markup](https://www.w3.org/TR/html-polyglot/))
-   <span class="nowrap">`html4`</span> ([XHTML](https://www.w3.org/TR/xhtml1/) 1.0 Transitional)
-   <span class="nowrap">`icml`</span> ([InDesign ICML](https://manualzz.com/doc/9627253/adobe-indesign-cs6-idml-cookbook))
-   <span class="nowrap">`ipynb`</span> ([Jupyter notebook](https://nbformat.readthedocs.io/en/latest/))
-   <span class="nowrap">`jats_archiving`</span> ([JATS](https://jats.nlm.nih.gov) XML, Archiving and Interchange Tag Set)
-   <span class="nowrap">`jats_articleauthoring`</span> ([JATS](https://jats.nlm.nih.gov) XML, Article Authoring Tag Set)
-   <span class="nowrap">`jats_publishing`</span> ([JATS](https://jats.nlm.nih.gov) XML, Journal Publishing Tag Set)
-   <span class="nowrap">`jats`</span> (alias for <span class="nowrap">`jats_archiving`</span>)
-   <span class="nowrap">`jira`</span> ([Jira](https://jira.atlassian.com/secure/WikiRendererHelpAction.jspa?section=all)/Confluence wiki markup)
-   <span class="nowrap">`json`</span> (JSON version of native AST)
-   <span class="nowrap">`latex`</span> ([LaTeX](https://www.latex-project.org/))
-   <span class="nowrap">`man`</span> ([roff man](https://man.cx/groff_man(7)))
-   <span class="nowrap">`markdown`</span> ([Pandoc’s Markdown](#pandocs-markdown))
-   <span class="nowrap">`markdown_mmd`</span> ([MultiMarkdown](https://fletcherpenney.net/multimarkdown/))
-   <span class="nowrap">`markdown_phpextra`</span> ([PHP Markdown Extra](https://michelf.ca/projects/php-markdown/extra/))
-   <span class="nowrap">`markdown_strict`</span> (original unextended [Markdown](https://daringfireball.net/projects/markdown/))
-   <span class="nowrap">`markua`</span> ([Markua](https://leanpub.com/markua/read))
-   <span class="nowrap">`mediawiki`</span> ([MediaWiki markup](https://www.mediawiki.org/wiki/Help:Formatting))
-   <span class="nowrap">`ms`</span> ([roff ms](https://man.cx/groff_ms(7)))
-   <span class="nowrap">`muse`</span> ([Muse](https://amusewiki.org/library/manual))
-   <span class="nowrap">`native`</span> (native Haskell)
-   <span class="nowrap">`odt`</span> ([OpenOffice text document](https://en.wikipedia.org/wiki/OpenDocument))
-   <span class="nowrap">`opml`</span> ([OPML](http://dev.opml.org/spec2.html))
-   <span class="nowrap">`opendocument`</span> ([OpenDocument](http://opendocument.xml.org))
-   <span class="nowrap">`org`</span> ([Emacs Org mode](https://orgmode.org))
-   <span class="nowrap">`pdf`</span> ([PDF](https://www.adobe.com/pdf/))
-   <span class="nowrap">`plain`</span> (plain text)
-   <span class="nowrap">`pptx`</span> ([PowerPoint](https://en.wikipedia.org/wiki/Microsoft_PowerPoint) slide show)
-   <span class="nowrap">`rst`</span> ([reStructuredText](https://docutils.sourceforge.io/docs/ref/rst/introduction.html))
-   <span class="nowrap">`rtf`</span> ([Rich Text Format](https://en.wikipedia.org/wiki/Rich_Text_Format))
-   <span class="nowrap">`texinfo`</span> ([GNU Texinfo](https://www.gnu.org/software/texinfo/))
-   <span class="nowrap">`textile`</span> ([Textile](https://textile-lang.com))
-   <span class="nowrap">`slideous`</span> ([Slideous](https://goessner.net/articles/slideous/) HTML and JavaScript slide show)
-   <span class="nowrap">`slidy`</span> ([Slidy](https://www.w3.org/Talks/Tools/Slidy2/) HTML and JavaScript slide show)
-   <span class="nowrap">`dzslides`</span> ([DZSlides](https://paulrouget.com/dzslides/) HTML5 + JavaScript slide show)
-   <span class="nowrap">`revealjs`</span> ([reveal.js](https://revealjs.com/) HTML5 + JavaScript slide show)
-   <span class="nowrap">`s5`</span> ([S5](https://meyerweb.com/eric/tools/s5/) HTML and JavaScript slide show)
-   <span class="nowrap">`tei`</span> ([TEI Simple](https://github.com/TEIC/TEI-Simple))
-   <span class="nowrap">`typst`</span> ([typst](https://typst.app))
-   <span class="nowrap">`xwiki`</span> ([XWiki markup](https://www.xwiki.org/xwiki/bin/view/Documentation/UserGuide/Features/XWikiSyntax/))
-   <span class="nowrap">`zimwiki`</span> ([ZimWiki markup](https://zim-wiki.org/manual/Help/Wiki_Syntax.html))
-   the path of a custom Lua writer, see [Custom readers and writers](#custom-readers-and-writers) below

Note that <span class="nowrap">`odt`</span>, <span class="nowrap">`docx`</span>, <span class="nowrap">`epub`</span>, and <span class="nowrap">`pdf`</span> output will not be directed to *stdout* unless forced with <a href="#option--output" class="option"><span class="nowrap"><code>-o -</code></span></a>.

Extensions can be individually enabled or disabled by appending <span class="nowrap">`+EXTENSION`</span> or <span class="nowrap">`-EXTENSION`</span> to the format name. See [Extensions](#extensions) below, for a list of extensions and their names. See <a href="#option--list-output-formats" class="option"><span class="nowrap"><code>--list-output-formats</code></span></a> and <a href="#option--list-extensions" class="option"><span class="nowrap"><code>--list-extensions</code></span></a>, below.

<span id="option--output" class="option-anchor"><span class="nowrap">`-o`</span> *FILE*, <span class="nowrap">`--output=`</span>*FILE*</span>  
Write output to *FILE* instead of *stdout*. If *FILE* is <span class="nowrap">`-`</span>, output will go to *stdout*, even if a non-textual format (<span class="nowrap">`docx`</span>, <span class="nowrap">`odt`</span>, <span class="nowrap">`epub2`</span>, <span class="nowrap">`epub3`</span>) is specified. If the output format is <span class="nowrap">`chunkedhtml`</span> and *FILE* has no extension, then instead of producing a <span class="nowrap">`.zip`</span> file pandoc will create a directory *FILE* and unpack the zip archive there (unless *FILE* already exists, in which case an error will be raised).

<span id="option--data-dir" class="option-anchor"><span class="nowrap">`--data-dir=`</span>*DIRECTORY*</span>  
Specify the user data directory to search for pandoc data files. If this option is not specified, the default user data directory will be used. On \*nix and macOS systems this will be the <span class="nowrap">`pandoc`</span> subdirectory of the XDG data directory (by default, <span class="nowrap">`$HOME/.local/share`</span>, overridable by setting the <span class="nowrap">`XDG_DATA_HOME`</span> environment variable). If that directory does not exist and <span class="nowrap">`$HOME/.pandoc`</span> exists, it will be used (for backwards compatibility). On Windows the default user data directory is <span class="nowrap">`%APPDATA%\pandoc`</span>. You can find the default user data directory on your system by looking at the output of <span class="nowrap">`pandoc --version`</span>. Data files placed in this directory (for example, <span class="nowrap">`reference.odt`</span>, <span class="nowrap">`reference.docx`</span>, <span class="nowrap">`epub.css`</span>, <span class="nowrap">`templates`</span>) will override pandoc’s normal defaults. (Note that the user data directory is not created by pandoc, so you will need to create it yourself if you want to make use of it.)

<span id="option--defaults" class="option-anchor"><span class="nowrap">`-d`</span> *FILE*, <span class="nowrap">`--defaults=`</span>*FILE*</span>  
Specify a set of default option settings. *FILE* is a YAML file whose fields correspond to command-line option settings. All options for document conversion, including input and output files, can be set using a defaults file. The file will be searched for first in the working directory, and then in the <span class="nowrap">`defaults`</span> subdirectory of the user data directory (see <a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>). The <span class="nowrap">`.yaml`</span> extension may be omitted. See the section [Defaults files](#defaults-files) for more information on the file format. Settings from the defaults file may be overridden or extended by subsequent options on the command line.

<span id="option--bash-completion" class="option-anchor"><span class="nowrap">`--bash-completion`</span></span>  
Generate a bash completion script. To enable bash completion with pandoc, add this to your <span class="nowrap">`.bashrc`</span>:

    eval "$(pandoc --bash-completion)"

<span id="option--verbose" class="option-anchor"><span class="nowrap">`--verbose`</span></span>  
Give verbose debugging output.

<span id="option--quiet" class="option-anchor"><span class="nowrap">`--quiet`</span></span>  
Suppress warning messages.

<span id="option--fail-if-warnings[" class="option-anchor"><span class="nowrap">`--fail-if-warnings[=true|false]`</span></span>  
Exit with error status if there are any warnings.

<span id="option--log" class="option-anchor"><span class="nowrap">`--log=`</span>*FILE*</span>  
Write log messages in machine-readable JSON format to *FILE*. All messages above DEBUG level will be written, regardless of verbosity settings (<a href="#option--verbose" class="option"><span class="nowrap"><code>--verbose</code></span></a>, <a href="#option--quiet" class="option"><span class="nowrap"><code>--quiet</code></span></a>).

<span id="option--list-input-formats" class="option-anchor"><span class="nowrap">`--list-input-formats`</span></span>  
List supported input formats, one per line.

<span id="option--list-output-formats" class="option-anchor"><span class="nowrap">`--list-output-formats`</span></span>  
List supported output formats, one per line.

<span id="option--list-extensions" class="option-anchor"><span class="nowrap">`--list-extensions`</span>\[<span class="nowrap">`=`</span>*FORMAT*\]</span>  
List supported extensions for *FORMAT*, one per line, preceded by a <span class="nowrap">`+`</span> or <span class="nowrap">`-`</span> indicating whether it is enabled by default in *FORMAT*. If *FORMAT* is not specified, defaults for pandoc’s Markdown are given.

<span id="option--list-highlight-languages" class="option-anchor"><span class="nowrap">`--list-highlight-languages`</span></span>  
List supported languages for syntax highlighting, one per line.

<span id="option--list-highlight-styles" class="option-anchor"><span class="nowrap">`--list-highlight-styles`</span></span>  
List supported styles for syntax highlighting, one per line. See <a href="#option--highlight-style" class="option"><span class="nowrap"><code>--highlight-style</code></span></a>.

<span id="option--version" class="option-anchor"><span class="nowrap">`-v`</span>, <span class="nowrap">`--version`</span></span>  
Print version.

<span id="option--help" class="option-anchor"><span class="nowrap">`-h`</span>, <span class="nowrap">`--help`</span></span>  
Show usage message.

<a href="#reader-options" class="anchor"></a>Reader options <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------

<span id="option--shift-heading-level-by" class="option-anchor"><span class="nowrap">`--shift-heading-level-by=`</span>*NUMBER*</span>  
Shift heading levels by a positive or negative integer. For example, with <a href="#option--shift-heading-level-by" class="option"><span class="nowrap"><code>--shift-heading-level-by=-1</code></span></a>, level 2 headings become level 1 headings, and level 3 headings become level 2 headings. Headings cannot have a level less than 1, so a heading that would be shifted below level 1 becomes a regular paragraph. Exception: with a shift of -N, a level-N heading at the beginning of the document replaces the metadata title. <a href="#option--shift-heading-level-by" class="option"><span class="nowrap"><code>--shift-heading-level-by=-1</code></span></a> is a good choice when converting HTML or Markdown documents that use an initial level-1 heading for the document title and level-2+ headings for sections. <a href="#option--shift-heading-level-by" class="option"><span class="nowrap"><code>--shift-heading-level-by=1</code></span></a> may be a good choice for converting Markdown documents that use level-1 headings for sections to HTML, since pandoc uses a level-1 heading to render the document title.

<span id="option--base-header-level" class="option-anchor"><span class="nowrap">`--base-header-level=`</span>*NUMBER*</span>  
*Deprecated. Use <a href="#option--shift-heading-level-by" class="option"><span class="nowrap"><code>--shift-heading-level-by</code></span></a>=X instead, where X = NUMBER - 1.* Specify the base level for headings (defaults to 1).

<span id="option--indented-code-classes" class="option-anchor"><span class="nowrap">`--indented-code-classes=`</span>*CLASSES*</span>  
Specify classes to use for indented code blocks—for example, <span class="nowrap">`perl,numberLines`</span> or <span class="nowrap">`haskell`</span>. Multiple classes may be separated by spaces or commas.

<span id="option--default-image-extension" class="option-anchor"><span class="nowrap">`--default-image-extension=`</span>*EXTENSION*</span>  
Specify a default extension to use when image paths/URLs have no extension. This allows you to use the same source for formats that require different kinds of images. Currently this option only affects the Markdown and LaTeX readers.

<span id="option--file-scope[" class="option-anchor"><span class="nowrap">`--file-scope[=true|false]`</span></span>  
Parse each file individually before combining for multifile documents. This will allow footnotes in different files with the same identifiers to work as expected. If this option is set, footnotes and links will not work across files. Reading binary files (docx, odt, epub) implies <a href="#option--file-scope%5B" class="option"><span class="nowrap"><code>--file-scope</code></span></a>.

If two or more files are processed using <a href="#option--file-scope%5B" class="option"><span class="nowrap"><code>--file-scope</code></span></a>, prefixes based on the filenames will be added to identifiers in order to disambiguate them, and internal links will be adjusted accordingly. For example, a header with identifier <span class="nowrap">`foo`</span> in <span class="nowrap">`subdir/file1.txt`</span> will have its identifier changed to <span class="nowrap">`subdir__file1.txt__foo`</span>.

<span id="option--filter" class="option-anchor"><span class="nowrap">`-F`</span> *PROGRAM*, <span class="nowrap">`--filter=`</span>*PROGRAM*</span>  
Specify an executable to be used as a filter transforming the pandoc AST after the input is parsed and before the output is written. The executable should read JSON from stdin and write JSON to stdout. The JSON must be formatted like pandoc’s own JSON input and output. The name of the output format will be passed to the filter as the first argument. Hence,

    pandoc --filter ./caps.py -t latex

is equivalent to

    pandoc -t json | ./caps.py latex | pandoc -f json -t latex

The latter form may be useful for debugging filters.

Filters may be written in any language. <span class="nowrap">`Text.Pandoc.JSON`</span> exports <span class="nowrap">`toJSONFilter`</span> to facilitate writing filters in Haskell. Those who would prefer to write filters in python can use the module [<span class="nowrap">`pandocfilters`</span>](https://github.com/jgm/pandocfilters), installable from PyPI. There are also pandoc filter libraries in [PHP](https://github.com/vinai/pandocfilters-php), [perl](https://metacpan.org/pod/Pandoc::Filter), and [JavaScript/node.js](https://github.com/mvhenderson/pandoc-filter-node).

In order of preference, pandoc will look for filters in

1.  a specified full or relative path (executable or non-executable),

2.  <span class="nowrap">`$DATADIR/filters`</span> (executable or non-executable) where <span class="nowrap">`$DATADIR`</span> is the user data directory (see <a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>, above),

3.  <span class="nowrap">`$PATH`</span> (executable only).

Filters, Lua-filters, and citeproc processing are applied in the order specified on the command line.

<span id="option--lua-filter" class="option-anchor"><span class="nowrap">`-L`</span> *SCRIPT*, <span class="nowrap">`--lua-filter=`</span>*SCRIPT*</span>  
Transform the document in a similar fashion as JSON filters (see <a href="#option--filter" class="option"><span class="nowrap"><code>--filter</code></span></a>), but use pandoc’s built-in Lua filtering system. The given Lua script is expected to return a list of Lua filters which will be applied in order. Each Lua filter must contain element-transforming functions indexed by the name of the AST element on which the filter function should be applied.

The <span class="nowrap">`pandoc`</span> Lua module provides helper functions for element creation. It is always loaded into the script’s Lua environment.

See the [Lua filters documentation](https://pandoc.org/lua-filters.html) for further details.

In order of preference, pandoc will look for Lua filters in

1.  a specified full or relative path,

2.  <span class="nowrap">`$DATADIR/filters`</span> where <span class="nowrap">`$DATADIR`</span> is the user data directory (see <a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>, above).

Filters, Lua filters, and citeproc processing are applied in the order specified on the command line.

<span id="option--metadata" class="option-anchor"><span class="nowrap">`-M`</span> *KEY*\[<span class="nowrap">`=`</span>*VAL*\], <span class="nowrap">`--metadata=`</span>*KEY*\[<span class="nowrap">`:`</span>*VAL*\]</span>  
Set the metadata field *KEY* to the value *VAL*. A value specified on the command line overrides a value specified in the document using [YAML metadata blocks](#extension-yaml_metadata_block). Values will be parsed as YAML boolean or string values. If no value is specified, the value will be treated as Boolean true. Like <a href="#option--variable" class="option"><span class="nowrap"><code>--variable</code></span></a>, <a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata</code></span></a> causes template variables to be set. But unlike <a href="#option--variable" class="option"><span class="nowrap"><code>--variable</code></span></a>, <a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata</code></span></a> affects the metadata of the underlying document (which is accessible from filters and may be printed in some output formats) and metadata values will be escaped when inserted into the template.

<span id="option--metadata-file" class="option-anchor"><span class="nowrap">`--metadata-file=`</span>*FILE*</span>  
Read metadata from the supplied YAML (or JSON) file. This option can be used with every input format, but string scalars in the metadata file will always be parsed as Markdown. (If the input format is Markdown or a Markdown variant, then the same variant will be used to parse the metadata file; if it is a non-Markdown format, pandoc’s default Markdown extensions will be used.) This option can be used repeatedly to include multiple metadata files; values in files specified later on the command line will be preferred over those specified in earlier files. Metadata values specified inside the document, or by using <a href="#option--metadata" class="option"><span class="nowrap"><code>-M</code></span></a>, overwrite values specified with this option. The file will be searched for first in the working directory, and then in the <span class="nowrap">`metadata`</span> subdirectory of the user data directory (see <a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>).

<span id="option--preserve-tabs[" class="option-anchor"><span class="nowrap">`-p`</span>, <span class="nowrap">`--preserve-tabs[=true|false]`</span></span>  
Preserve tabs instead of converting them to spaces. (By default, pandoc converts tabs to spaces before parsing its input.) Note that this will only affect tabs in literal code spans and code blocks. Tabs in regular text are always treated as spaces.

<span id="option--tab-stop" class="option-anchor"><span class="nowrap">`--tab-stop=`</span>*NUMBER*</span>  
Specify the number of spaces per tab (default is 4).

<span id="option--track-changes" class="option-anchor"><span class="nowrap">`--track-changes=accept`</span>|<span class="nowrap">`reject`</span>|<span class="nowrap">`all`</span></span>  
Specifies what to do with insertions, deletions, and comments produced by the MS Word “Track Changes” feature. <span class="nowrap">`accept`</span> (the default) processes all the insertions and deletions. <span class="nowrap">`reject`</span> ignores them. Both <span class="nowrap">`accept`</span> and <span class="nowrap">`reject`</span> ignore comments. <span class="nowrap">`all`</span> includes all insertions, deletions, and comments, wrapped in spans with <span class="nowrap">`insertion`</span>, <span class="nowrap">`deletion`</span>, <span class="nowrap">`comment-start`</span>, and <span class="nowrap">`comment-end`</span> classes, respectively. The author and time of change is included. <span class="nowrap">`all`</span> is useful for scripting: only accepting changes from a certain reviewer, say, or before a certain date. If a paragraph is inserted or deleted, <span class="nowrap">`track-changes=all`</span> produces a span with the class <span class="nowrap">`paragraph-insertion`</span>/<span class="nowrap">`paragraph-deletion`</span> before the affected paragraph break. This option only affects the docx reader.

<span id="option--extract-media" class="option-anchor"><span class="nowrap">`--extract-media=`</span>*DIR*</span>  
Extract images and other media contained in or linked from the source document to the path *DIR*, creating it if necessary, and adjust the images references in the document so they point to the extracted files. Media are downloaded, read from the file system, or extracted from a binary container (e.g. docx), as needed. The original file paths are used if they are relative paths not containing <span class="nowrap">`..`</span>. Otherwise filenames are constructed from the SHA1 hash of the contents.

<span id="option--abbreviations" class="option-anchor"><span class="nowrap">`--abbreviations=`</span>*FILE*</span>  
Specifies a custom abbreviations file, with abbreviations one to a line. If this option is not specified, pandoc will read the data file <span class="nowrap">`abbreviations`</span> from the user data directory or fall back on a system default. To see the system default, use <span class="nowrap">`pandoc --print-default-data-file=abbreviations`</span>. The only use pandoc makes of this list is in the Markdown reader. Strings found in this list will be followed by a nonbreaking space, and the period will not produce sentence-ending space in formats like LaTeX. The strings may not contain spaces.

<span id="option--trace[" class="option-anchor"><span class="nowrap">`--trace[=true|false]`</span></span>  
Print diagnostic output tracing parser progress to stderr. This option is intended for use by developers in diagnosing performance issues.

<a href="#general-writer-options" class="anchor"></a>General writer options <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------------------------

<span id="option--standalone" class="option-anchor"><span class="nowrap">`-s`</span>, <span class="nowrap">`--standalone`</span></span>  
Produce output with an appropriate header and footer (e.g. a standalone HTML, LaTeX, TEI, or RTF file, not a fragment). This option is set automatically for <span class="nowrap">`pdf`</span>, <span class="nowrap">`epub`</span>, <span class="nowrap">`epub3`</span>, <span class="nowrap">`fb2`</span>, <span class="nowrap">`docx`</span>, and <span class="nowrap">`odt`</span> output. For <span class="nowrap">`native`</span> output, this option causes metadata to be included; otherwise, metadata is suppressed.

<span id="option--template" class="option-anchor"><span class="nowrap">`--template=`</span>*FILE*|*URL*</span>  
Use the specified file as a custom template for the generated document. Implies <a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>. See [Templates](#templates), below, for a description of template syntax. If no extension is specified, an extension corresponding to the writer will be added, so that <a href="#option--template" class="option"><span class="nowrap"><code>--template=special</code></span></a> looks for <span class="nowrap">`special.html`</span> for HTML output. If the template is not found, pandoc will search for it in the <span class="nowrap">`templates`</span> subdirectory of the user data directory (see <a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>). If this option is not used, a default template appropriate for the output format will be used (see <a href="#option--print-default-template" class="option"><span class="nowrap"><code>-D/--print-default-template</code></span></a>).

<span id="option--variable" class="option-anchor"><span class="nowrap">`-V`</span> *KEY*\[<span class="nowrap">`=`</span>*VAL*\], <span class="nowrap">`--variable=`</span>*KEY*\[<span class="nowrap">`:`</span>*VAL*\]</span>  
Set the template variable *KEY* to the value *VAL* when rendering the document in standalone mode. If no *VAL* is specified, the key will be given the value <span class="nowrap">`true`</span>.

<span id="option--sandbox[" class="option-anchor"><span class="nowrap">`--sandbox[=true|false]`</span></span>  
Run pandoc in a sandbox, limiting IO operations in readers and writers to reading the files specified on the command line. Note that this option does not limit IO operations by filters or in the production of PDF documents. But it does offer security against, for example, disclosure of files through the use of <span class="nowrap">`include`</span> directives. Anyone using pandoc on untrusted user input should use this option.

Note: some readers and writers (e.g., <span class="nowrap">`docx`</span>) need access to data files. If these are stored on the file system, then pandoc will not be able to find them when run in <a href="#option--sandbox%5B" class="option"><span class="nowrap"><code>--sandbox</code></span></a> mode and will raise an error. For these applications, we recommend using a pandoc binary compiled with the <span class="nowrap">`embed_data_files`</span> option, which causes the data files to be baked into the binary instead of being stored on the file system.

<span id="option--print-default-template" class="option-anchor"><span class="nowrap">`-D`</span> *FORMAT*, <span class="nowrap">`--print-default-template=`</span>*FORMAT*</span>  
Print the system default template for an output *FORMAT*. (See <a href="#option--to" class="option"><span class="nowrap"><code>-t</code></span></a> for a list of possible *FORMAT*s.) Templates in the user data directory are ignored. This option may be used with <a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>/<a href="#option--output" class="option"><span class="nowrap"><code>--output</code></span></a> to redirect output to a file, but <a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>/<a href="#option--output" class="option"><span class="nowrap"><code>--output</code></span></a> must come before <a href="#option--print-default-template" class="option"><span class="nowrap"><code>--print-default-template</code></span></a> on the command line.

Note that some of the default templates use partials, for example <span class="nowrap">`styles.html`</span>. To print the partials, use <a href="#option--print-default-data-file" class="option"><span class="nowrap"><code>--print-default-data-file</code></span></a>: for example, <a href="#option--print-default-data-file" class="option"><span class="nowrap"><code>--print-default-data-file=templates/styles.html</code></span></a>.

<span id="option--print-default-data-file" class="option-anchor"><span class="nowrap">`--print-default-data-file=`</span>*FILE*</span>  
Print a system default data file. Files in the user data directory are ignored. This option may be used with <a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>/<a href="#option--output" class="option"><span class="nowrap"><code>--output</code></span></a> to redirect output to a file, but <a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>/<a href="#option--output" class="option"><span class="nowrap"><code>--output</code></span></a> must come before <a href="#option--print-default-data-file" class="option"><span class="nowrap"><code>--print-default-data-file</code></span></a> on the command line.

<span id="option--eol" class="option-anchor"><span class="nowrap">`--eol=crlf`</span>|<span class="nowrap">`lf`</span>|<span class="nowrap">`native`</span></span>  
Manually specify line endings: <span class="nowrap">`crlf`</span> (Windows), <span class="nowrap">`lf`</span> (macOS/Linux/UNIX), or <span class="nowrap">`native`</span> (line endings appropriate to the OS on which pandoc is being run). The default is <span class="nowrap">`native`</span>.

<span id="option--dpi" class="option-anchor"><span class="nowrap">`--dpi`</span>=*NUMBER*</span>  
Specify the default dpi (dots per inch) value for conversion from pixels to inch/centimeters and vice versa. (Technically, the correct term would be ppi: pixels per inch.) The default is 96dpi. When images contain information about dpi internally, the encoded value is used instead of the default specified by this option.

<span id="option--wrap" class="option-anchor"><span class="nowrap">`--wrap=auto`</span>|<span class="nowrap">`none`</span>|<span class="nowrap">`preserve`</span></span>  
Determine how text is wrapped in the output (the source code, not the rendered version). With <span class="nowrap">`auto`</span> (the default), pandoc will attempt to wrap lines to the column width specified by <a href="#option--columns" class="option"><span class="nowrap"><code>--columns</code></span></a> (default 72). With <span class="nowrap">`none`</span>, pandoc will not wrap lines at all. With <span class="nowrap">`preserve`</span>, pandoc will attempt to preserve the wrapping from the source document (that is, where there are nonsemantic newlines in the source, there will be nonsemantic newlines in the output as well). In <span class="nowrap">`ipynb`</span> output, this option affects wrapping of the contents of Markdown cells.

<span id="option--columns" class="option-anchor"><span class="nowrap">`--columns=`</span>*NUMBER*</span>  
Specify length of lines in characters. This affects text wrapping in the generated source code (see <a href="#option--wrap" class="option"><span class="nowrap"><code>--wrap</code></span></a>). It also affects calculation of column widths for plain text tables (see [Tables](#tables) below).

<span id="option--toc[" class="option-anchor"><span class="nowrap">`--toc[=true|false]`</span>, <span class="nowrap">`--table-of-contents[=true|false]`</span></span>  
Include an automatically generated table of contents (or, in the case of <span class="nowrap">`latex`</span>, <span class="nowrap">`context`</span>, <span class="nowrap">`docx`</span>, <span class="nowrap">`odt`</span>, <span class="nowrap">`opendocument`</span>, <span class="nowrap">`rst`</span>, or <span class="nowrap">`ms`</span>, an instruction to create one) in the output document. This option has no effect unless <a href="#option--standalone" class="option"><span class="nowrap"><code>-s/--standalone</code></span></a> is used, and it has no effect on <span class="nowrap">`man`</span>, <span class="nowrap">`docbook4`</span>, <span class="nowrap">`docbook5`</span>, or <span class="nowrap">`jats`</span> output.

Note that if you are producing a PDF via <span class="nowrap">`ms`</span>, the table of contents will appear at the beginning of the document, before the title. If you would prefer it to be at the end of the document, use the option <a href="#option--pdf-engine-opt" class="option"><span class="nowrap"><code>--pdf-engine-opt=--no-toc-relocation</code></span></a>.

<span id="option--toc-depth" class="option-anchor"><span class="nowrap">`--toc-depth=`</span>*NUMBER*</span>  
Specify the number of section levels to include in the table of contents. The default is 3 (which means that level-1, 2, and 3 headings will be listed in the contents).

<span id="option--lof[" class="option-anchor"><span class="nowrap">`--lof[=true|false]`</span>, <span class="nowrap">`--list-of-figures[=true|false]`</span></span>  
Include an automatically generated list of figures (or, in some formats, an instruction to create one) in the output document. This option has no effect unless <a href="#option--standalone" class="option"><span class="nowrap"><code>-s/--standalone</code></span></a> is used, and it only has an effect on <span class="nowrap">`latex`</span>, <span class="nowrap">`context`</span>, and <span class="nowrap">`docx`</span> output.

<span id="option--lot[" class="option-anchor"><span class="nowrap">`--lot[=true|false]`</span>, <span class="nowrap">`--list-of-tables[=true|false]`</span></span>  
Include an automatically generated list of tables (or, in some formats, an instruction to create one) in the output document. This option has no effect unless <a href="#option--standalone" class="option"><span class="nowrap"><code>-s/--standalone</code></span></a> is used, and it only has an effect on <span class="nowrap">`latex`</span>, <span class="nowrap">`context`</span>, and <span class="nowrap">`docx`</span> output.

<span id="option--strip-comments[" class="option-anchor"><span class="nowrap">`--strip-comments[=true|false]`</span></span>  
Strip out HTML comments in the Markdown or Textile source, rather than passing them on to Markdown, Textile or HTML output as raw HTML. This does not apply to HTML comments inside raw HTML blocks when the <span class="nowrap">`markdown_in_html_blocks`</span> extension is not set.

<span id="option--no-highlight" class="option-anchor"><span class="nowrap">`--no-highlight`</span></span>  
Disables syntax highlighting for code blocks and inlines, even when a language attribute is given.

<span id="option--highlight-style" class="option-anchor"><span class="nowrap">`--highlight-style=`</span>*STYLE*|*FILE*</span>  
Specifies the coloring style to be used in highlighted source code. Options are <span class="nowrap">`pygments`</span> (the default), <span class="nowrap">`kate`</span>, <span class="nowrap">`monochrome`</span>, <span class="nowrap">`breezeDark`</span>, <span class="nowrap">`espresso`</span>, <span class="nowrap">`zenburn`</span>, <span class="nowrap">`haddock`</span>, and <span class="nowrap">`tango`</span>. For more information on syntax highlighting in pandoc, see [Syntax highlighting](#syntax-highlighting), below. See also <a href="#option--list-highlight-styles" class="option"><span class="nowrap"><code>--list-highlight-styles</code></span></a>.

Instead of a *STYLE* name, a JSON file with extension <span class="nowrap">`.theme`</span> may be supplied. This will be parsed as a KDE syntax highlighting theme and (if valid) used as the highlighting style.

To generate the JSON version of an existing style, use <a href="#option--print-highlight-style" class="option"><span class="nowrap"><code>--print-highlight-style</code></span></a>.

<span id="option--print-highlight-style" class="option-anchor"><span class="nowrap">`--print-highlight-style=`</span>*STYLE*|*FILE*</span>  
Prints a JSON version of a highlighting style, which can be modified, saved with a <span class="nowrap">`.theme`</span> extension, and used with <a href="#option--highlight-style" class="option"><span class="nowrap"><code>--highlight-style</code></span></a>. This option may be used with <a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>/<a href="#option--output" class="option"><span class="nowrap"><code>--output</code></span></a> to redirect output to a file, but <a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a>/<a href="#option--output" class="option"><span class="nowrap"><code>--output</code></span></a> must come before <a href="#option--print-highlight-style" class="option"><span class="nowrap"><code>--print-highlight-style</code></span></a> on the command line.

<span id="option--syntax-definition" class="option-anchor"><span class="nowrap">`--syntax-definition=`</span>*FILE*</span>  
Instructs pandoc to load a KDE XML syntax definition file, which will be used for syntax highlighting of appropriately marked code blocks. This can be used to add support for new languages or to use altered syntax definitions for existing languages. This option may be repeated to add multiple syntax definitions.

<span id="option--include-in-header" class="option-anchor"><span class="nowrap">`-H`</span> *FILE*, <span class="nowrap">`--include-in-header=`</span>*FILE*|*URL*</span>  
Include contents of *FILE*, verbatim, at the end of the header. This can be used, for example, to include special CSS or JavaScript in HTML documents. This option can be used repeatedly to include multiple files in the header. They will be included in the order specified. Implies <a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>.

<span id="option--include-before-body" class="option-anchor"><span class="nowrap">`-B`</span> *FILE*, <span class="nowrap">`--include-before-body=`</span>*FILE*|*URL*</span>  
Include contents of *FILE*, verbatim, at the beginning of the document body (e.g. after the <span class="nowrap">`<body>`</span> tag in HTML, or the <span class="nowrap">`\begin{document}`</span> command in LaTeX). This can be used to include navigation bars or banners in HTML documents. This option can be used repeatedly to include multiple files. They will be included in the order specified. Implies <a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>. Note that if the output format is <span class="nowrap">`odt`</span>, this file must be in OpenDocument XML format suitable for insertion into the body of the document, and if the output is <span class="nowrap">`docx`</span>, this file must be in appropriate OpenXML format.

<span id="option--include-after-body" class="option-anchor"><span class="nowrap">`-A`</span> *FILE*, <span class="nowrap">`--include-after-body=`</span>*FILE*|*URL*</span>  
Include contents of *FILE*, verbatim, at the end of the document body (before the <span class="nowrap">`</body>`</span> tag in HTML, or the <span class="nowrap">`\end{document}`</span> command in LaTeX). This option can be used repeatedly to include multiple files. They will be included in the order specified. Implies <a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>. Note that if the output format is <span class="nowrap">`odt`</span>, this file must be in OpenDocument XML format suitable for insertion into the body of the document, and if the output is <span class="nowrap">`docx`</span>, this file must be in appropriate OpenXML format.

<span id="option--resource-path" class="option-anchor"><span class="nowrap">`--resource-path=`</span>*SEARCHPATH*</span>  
List of paths to search for images and other resources. The paths should be separated by <span class="nowrap">`:`</span> on Linux, UNIX, and macOS systems, and by <span class="nowrap">`;`</span> on Windows. If <a href="#option--resource-path" class="option"><span class="nowrap"><code>--resource-path</code></span></a> is not specified, the default resource path is the working directory. Note that, if <a href="#option--resource-path" class="option"><span class="nowrap"><code>--resource-path</code></span></a> is specified, the working directory must be explicitly listed or it will not be searched. For example: <a href="#option--resource-path" class="option"><span class="nowrap"><code>--resource-path=.:test</code></span></a> will search the working directory and the <span class="nowrap">`test`</span> subdirectory, in that order. This option can be used repeatedly. Search path components that come later on the command line will be searched before those that come earlier, so <a href="#option--resource-path" class="option"><span class="nowrap"><code>--resource-path foo:bar --resource-path baz:bim</code></span></a> is equivalent to <a href="#option--resource-path" class="option"><span class="nowrap"><code>--resource-path baz:bim:foo:bar</code></span></a>. Note that this option only has an effect when pandoc itself needs to find an image (e.g., in producing a PDF or docx, or when <a href="#option--embed-resources%5B" class="option"><span class="nowrap"><code>--embed-resources</code></span></a> is used.) It will not cause image paths to be rewritten in other cases (e.g., when pandoc is generating LaTeX or HTML).

<span id="option--request-header" class="option-anchor"><span class="nowrap">`--request-header=`</span>*NAME*<span class="nowrap">`:`</span>*VAL*</span>  
Set the request header *NAME* to the value *VAL* when making HTTP requests (for example, when a URL is given on the command line, or when resources used in a document must be downloaded). If you’re behind a proxy, you also need to set the environment variable <span class="nowrap">`http_proxy`</span> to <span class="nowrap">`http://...`</span>.

<span id="option--no-check-certificate[" class="option-anchor"><span class="nowrap">`--no-check-certificate[=true|false]`</span></span>  
Disable the certificate verification to allow access to unsecure HTTP resources (for example when the certificate is no longer valid or self signed).

<a href="#options-affecting-specific-writers" class="anchor"></a>Options affecting specific writers <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------------------------------------------------

<span id="option--self-contained[" class="option-anchor"><span class="nowrap">`--self-contained[=true|false]`</span></span>  
*Deprecated synonym for <a href="#option--embed-resources%5B" class="option"><span class="nowrap"><code>--embed-resources --standalone</code></span></a>.*

<span id="option--embed-resources[" class="option-anchor"><span class="nowrap">`--embed-resources[=true|false]`</span></span>  
Produce a standalone HTML file with no external dependencies, using <span class="nowrap">`data:`</span> URIs to incorporate the contents of linked scripts, stylesheets, images, and videos. The resulting file should be “self-contained,” in the sense that it needs no external files and no net access to be displayed properly by a browser. This option works only with HTML output formats, including <span class="nowrap">`html4`</span>, <span class="nowrap">`html5`</span>, <span class="nowrap">`html+lhs`</span>, <span class="nowrap">`html5+lhs`</span>, <span class="nowrap">`s5`</span>, <span class="nowrap">`slidy`</span>, <span class="nowrap">`slideous`</span>, <span class="nowrap">`dzslides`</span>, and <span class="nowrap">`revealjs`</span>. Scripts, images, and stylesheets at absolute URLs will be downloaded; those at relative URLs will be sought relative to the working directory (if the first source file is local) or relative to the base URL (if the first source file is remote). Elements with the attribute <span class="nowrap">`data-external="1"`</span> will be left alone; the documents they link to will not be incorporated in the document. Limitation: resources that are loaded dynamically through JavaScript cannot be incorporated; as a result, fonts may be missing when <a href="#option--mathjax" class="option"><span class="nowrap"><code>--mathjax</code></span></a> is used, and some advanced features (e.g. zoom or speaker notes) may not work in an offline “self-contained” <span class="nowrap">`reveal.js`</span> slide show.

For SVG images, <span class="nowrap">`img`</span> tags with <span class="nowrap">`data:`</span> URIs are used, unless the image has the class <span class="nowrap">`inline-svg`</span>, in which case an inline SVG element is inserted. This approach is recommended when there are many occurrences of the same SVG in a document, as <span class="nowrap">`<use>`</span> elements will be used to reduce duplication.

<span id="option--link-images[" class="option-anchor"><span class="nowrap">`--link-images[=true|false]`</span></span>  
Include links to images instead of embedding the images in ODT. (This option currently only affects ODT output.)

<span id="option--html-q-tags[" class="option-anchor"><span class="nowrap">`--html-q-tags[=true|false]`</span></span>  
Use <span class="nowrap">`<q>`</span> tags for quotes in HTML. (This option only has an effect if the <span class="nowrap">`smart`</span> extension is enabled for the input format used.)

<span id="option--ascii[" class="option-anchor"><span class="nowrap">`--ascii[=true|false]`</span></span>  
Use only ASCII characters in output. Currently supported for XML and HTML formats (which use entities instead of UTF-8 when this option is selected), CommonMark, gfm, and Markdown (which use entities), roff man and ms (which use hexadecimal escapes), and to a limited degree LaTeX (which uses standard commands for accented characters when possible).

<span id="option--reference-links[" class="option-anchor"><span class="nowrap">`--reference-links[=true|false]`</span></span>  
Use reference-style links, rather than inline links, in writing Markdown or reStructuredText. By default inline links are used. The placement of link references is affected by the <a href="#option--reference-location" class="option"><span class="nowrap"><code>--reference-location</code></span></a> option.

<span id="option--reference-location" class="option-anchor"><span class="nowrap">`--reference-location=block`</span>|<span class="nowrap">`section`</span>|<span class="nowrap">`document`</span></span>  
Specify whether footnotes (and references, if <span class="nowrap">`reference-links`</span> is set) are placed at the end of the current (top-level) block, the current section, or the document. The default is <span class="nowrap">`document`</span>. Currently this option only affects the <span class="nowrap">`markdown`</span>, <span class="nowrap">`muse`</span>, <span class="nowrap">`html`</span>, <span class="nowrap">`epub`</span>, <span class="nowrap">`slidy`</span>, <span class="nowrap">`s5`</span>, <span class="nowrap">`slideous`</span>, <span class="nowrap">`dzslides`</span>, and <span class="nowrap">`revealjs`</span> writers. In slide formats, specifying <a href="#option--reference-location" class="option"><span class="nowrap"><code>--reference-location=section</code></span></a> will cause notes to be rendered at the bottom of a slide.

<span id="option--figure-caption-position" class="option-anchor"><span class="nowrap">`--figure-caption-position=above`</span>|<span class="nowrap">`below`</span></span>  
Specify whether figure captions go above or below figures (default is <span class="nowrap">`below`</span>). This option only affects HTML, LaTeX, Docx, ODT, and Typst output.

<span id="option--table-caption-position" class="option-anchor"><span class="nowrap">`--table-caption-position=above`</span>|<span class="nowrap">`below`</span></span>  
Specify whether table captions go above or below tables (default is <span class="nowrap">`above`</span>). This option only affects HTML, LaTeX, Docx, ODT, and Typst output.

<span id="option--markdown-headings" class="option-anchor"><span class="nowrap">`--markdown-headings=setext`</span>|<span class="nowrap">`atx`</span></span>  
Specify whether to use ATX-style (<span class="nowrap">`#`</span>-prefixed) or Setext-style (underlined) headings for level 1 and 2 headings in Markdown output. (The default is <span class="nowrap">`atx`</span>.) ATX-style headings are always used for levels 3+. This option also affects Markdown cells in <span class="nowrap">`ipynb`</span> output.

<span id="option--list-tables[" class="option-anchor"><span class="nowrap">`--list-tables[=true|false]`</span></span>  
Render tables as list tables in RST output.

<span id="option--top-level-division" class="option-anchor"><span class="nowrap">`--top-level-division=default`</span>|<span class="nowrap">`section`</span>|<span class="nowrap">`chapter`</span>|<span class="nowrap">`part`</span></span>  
Treat top-level headings as the given division type in LaTeX, ConTeXt, DocBook, and TEI output. The hierarchy order is part, chapter, then section; all headings are shifted such that the top-level heading becomes the specified type. The default behavior is to determine the best division type via heuristics: unless other conditions apply, <span class="nowrap">`section`</span> is chosen. When the <span class="nowrap">`documentclass`</span> variable is set to <span class="nowrap">`report`</span>, <span class="nowrap">`book`</span>, or <span class="nowrap">`memoir`</span> (unless the <span class="nowrap">`article`</span> option is specified), <span class="nowrap">`chapter`</span> is implied as the setting for this option. If <span class="nowrap">`beamer`</span> is the output format, specifying either <span class="nowrap">`chapter`</span> or <span class="nowrap">`part`</span> will cause top-level headings to become <span class="nowrap">`\part{..}`</span>, while second-level headings remain as their default type.

<span id="option--number-sections" class="option-anchor"><span class="nowrap">`-N`</span>, <span class="nowrap">`--number-sections=[true|false]`</span></span>  
Number section headings in LaTeX, ConTeXt, HTML, Docx, ms, or EPUB output. By default, sections are not numbered. Sections with class <span class="nowrap">`unnumbered`</span> will never be numbered, even if <a href="#option--number-sections" class="option"><span class="nowrap"><code>--number-sections</code></span></a> is specified.

<span id="option--number-offset" class="option-anchor"><span class="nowrap">`--number-offset=`</span>*NUMBER*\[<span class="nowrap">`,`</span>*NUMBER*<span class="nowrap">`,`</span>*…*\]</span>  
Offsets for section heading numbers. The first number is added to the section number for level-1 headings, the second for level-2 headings, and so on. So, for example, if you want the first level-1 heading in your document to be numbered “6” instead of “1”, specify <a href="#option--number-offset" class="option"><span class="nowrap"><code>--number-offset=5</code></span></a>. If your document starts with a level-2 heading which you want to be numbered “1.5”, specify <a href="#option--number-offset" class="option"><span class="nowrap"><code>--number-offset=1,4</code></span></a>. <a href="#option--number-offset" class="option"><span class="nowrap"><code>--number-offset</code></span></a> only directly affects the number of the first section heading in a document; subsequent numbers increment in the normal way. Implies <a href="#option--number-sections" class="option"><span class="nowrap"><code>--number-sections</code></span></a>. Currently this feature only affects HTML and Docx output.

<span id="option--listings[" class="option-anchor"><span class="nowrap">`--listings[=true|false]`</span></span>  
Use the [<span class="nowrap">`listings`</span>](https://ctan.org/pkg/listings) package for LaTeX code blocks. The package does not support multi-byte encoding for source code. To handle UTF-8 you would need to use a custom template. This issue is fully documented here: [Encoding issue with the listings package](https://en.wikibooks.org/wiki/LaTeX/Source_Code_Listings#Encoding_issue).

<span id="option--incremental[" class="option-anchor"><span class="nowrap">`-i`</span>, <span class="nowrap">`--incremental[=true|false]`</span></span>  
Make list items in slide shows display incrementally (one by one). The default is for lists to be displayed all at once.

<span id="option--slide-level" class="option-anchor"><span class="nowrap">`--slide-level=`</span>*NUMBER*</span>  
Specifies that headings with the specified level create slides (for <span class="nowrap">`beamer`</span>, <span class="nowrap">`revealjs`</span>, <span class="nowrap">`pptx`</span>, <span class="nowrap">`s5`</span>, <span class="nowrap">`slidy`</span>, <span class="nowrap">`slideous`</span>, <span class="nowrap">`dzslides`</span>). Headings above this level in the hierarchy are used to divide the slide show into sections; headings below this level create subheads within a slide. Valid values are 0-6. If a slide level of 0 is specified, slides will not be split automatically on headings, and horizontal rules must be used to indicate slide boundaries. If a slide level is not specified explicitly, the slide level will be set automatically based on the contents of the document; see [Structuring the slide show](#structuring-the-slide-show).

<span id="option--section-divs[" class="option-anchor"><span class="nowrap">`--section-divs[=true|false]`</span></span>  
Wrap sections in <span class="nowrap">`<section>`</span> tags (or <span class="nowrap">`<div>`</span> tags for <span class="nowrap">`html4`</span>), and attach identifiers to the enclosing <span class="nowrap">`<section>`</span> (or <span class="nowrap">`<div>`</span>) rather than the heading itself (see [Heading identifiers](#heading-identifiers), below). This option only affects HTML output (and does not affect HTML slide formats).

<span id="option--email-obfuscation" class="option-anchor"><span class="nowrap">`--email-obfuscation=none`</span>|<span class="nowrap">`javascript`</span>|<span class="nowrap">`references`</span></span>  
Specify a method for obfuscating <span class="nowrap">`mailto:`</span> links in HTML documents. <span class="nowrap">`none`</span> leaves <span class="nowrap">`mailto:`</span> links as they are. <span class="nowrap">`javascript`</span> obfuscates them using JavaScript. <span class="nowrap">`references`</span> obfuscates them by printing their letters as decimal or hexadecimal character references. The default is <span class="nowrap">`none`</span>.

<span id="option--id-prefix" class="option-anchor"><span class="nowrap">`--id-prefix=`</span>*STRING*</span>  
Specify a prefix to be added to all identifiers and internal links in HTML and DocBook output, and to footnote numbers in Markdown and Haddock output. This is useful for preventing duplicate identifiers when generating fragments to be included in other pages.

<span id="option--title-prefix" class="option-anchor"><span class="nowrap">`-T`</span> *STRING*, <span class="nowrap">`--title-prefix=`</span>*STRING*</span>  
Specify *STRING* as a prefix at the beginning of the title that appears in the HTML header (but not in the title as it appears at the beginning of the HTML body). Implies <a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a>.

<span id="option--css" class="option-anchor"><span class="nowrap">`-c`</span> *URL*, <span class="nowrap">`--css=`</span>*URL*</span>  
Link to a CSS style sheet. This option can be used repeatedly to include multiple files. They will be included in the order specified. This option only affects HTML (including HTML slide shows) and EPUB output. It should be used together with <a href="#option--standalone" class="option"><span class="nowrap"><code>-s/--standalone</code></span></a>, because the link to the stylesheet goes in the document header.

A stylesheet is required for generating EPUB. If none is provided using this option (or the <span class="nowrap">`css`</span> or <span class="nowrap">`stylesheet`</span> metadata fields), pandoc will look for a file <span class="nowrap">`epub.css`</span> in the user data directory (see <a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>). If it is not found there, sensible defaults will be used.

<span id="option--reference-doc"><span class="nowrap">`--reference-doc=`</span>*FILE*|*URL*</span>  
Use the specified file as a style reference in producing a docx or ODT file.

Docx  
For best results, the reference docx should be a modified version of a docx file produced using pandoc. The contents of the reference docx are ignored, but its stylesheets and document properties (including margins, page size, header, and footer) are used in the new docx. If no reference docx is specified on the command line, pandoc will look for a file <span class="nowrap">`reference.docx`</span> in the user data directory (see <a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>). If this is not found either, sensible defaults will be used.

To produce a custom <span class="nowrap">`reference.docx`</span>, first get a copy of the default <span class="nowrap">`reference.docx`</span>: <span class="nowrap">`pandoc -o custom-reference.docx --print-default-data-file reference.docx`</span>. Then open <span class="nowrap">`custom-reference.docx`</span> in Word, modify the styles as you wish, and save the file. For best results, do not make changes to this file other than modifying the styles used by pandoc:

Paragraph styles:

-   Normal
-   Body Text
-   First Paragraph
-   Compact
-   Title
-   Subtitle
-   Author
-   Date
-   Abstract
-   AbstractTitle
-   Bibliography
-   Heading 1
-   Heading 2
-   Heading 3
-   Heading 4
-   Heading 5
-   Heading 6
-   Heading 7
-   Heading 8
-   Heading 9
-   Block Text \[for block quotes\]
-   Footnote Block Text \[for block quotes in footnotes\]
-   Source Code
-   Footnote Text
-   Definition Term
-   Definition
-   Caption
-   Table Caption
-   Image Caption
-   Figure
-   Captioned Figure
-   TOC Heading

Character styles:

-   Default Paragraph Font
-   Body Text Char
-   Verbatim Char
-   Footnote Reference
-   Hyperlink
-   Section Number

Table style:

-   Table

ODT  
For best results, the reference ODT should be a modified version of an ODT produced using pandoc. The contents of the reference ODT are ignored, but its stylesheets are used in the new ODT. If no reference ODT is specified on the command line, pandoc will look for a file <span class="nowrap">`reference.odt`</span> in the user data directory (see <a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>). If this is not found either, sensible defaults will be used.

To produce a custom <span class="nowrap">`reference.odt`</span>, first get a copy of the default <span class="nowrap">`reference.odt`</span>: <span class="nowrap">`pandoc -o custom-reference.odt --print-default-data-file reference.odt`</span>. Then open <span class="nowrap">`custom-reference.odt`</span> in LibreOffice, modify the styles as you wish, and save the file.

PowerPoint  
Templates included with Microsoft PowerPoint 2013 (either with <span class="nowrap">`.pptx`</span> or <span class="nowrap">`.potx`</span> extension) are known to work, as are most templates derived from these.

The specific requirement is that the template should contain layouts with the following names (as seen within PowerPoint):

-   Title Slide
-   Title and Content
-   Section Header
-   Two Content
-   Comparison
-   Content with Caption
-   Blank

For each name, the first layout found with that name will be used. If no layout is found with one of the names, pandoc will output a warning and use the layout with that name from the default reference doc instead. (How these layouts are used is described in [PowerPoint layout choice](#powerpoint-layout-choice).)

All templates included with a recent version of MS PowerPoint will fit these criteria. (You can click on <span class="nowrap">`Layout`</span> under the <span class="nowrap">`Home`</span> menu to check.)

You can also modify the default <span class="nowrap">`reference.pptx`</span>: first run <span class="nowrap">`pandoc -o custom-reference.pptx --print-default-data-file reference.pptx`</span>, and then modify <span class="nowrap">`custom-reference.pptx`</span> in MS PowerPoint (pandoc will use the layouts with the names listed above).

<span id="option--split-level" class="option-anchor"><span class="nowrap">`--split-level=`</span>*NUMBER*</span>  
Specify the heading level at which to split an EPUB or chunked HTML document into separate files. The default is to split into chapters at level-1 headings. In the case of EPUB, this option only affects the internal composition of the EPUB, not the way chapters and sections are displayed to users. Some readers may be slow if the chapter files are too large, so for large documents with few level-1 headings, one might want to use a chapter level of 2 or 3. For chunked HTML, this option determines how much content goes in each “chunk.”

<span id="option--chunk-template" class="option-anchor"><span class="nowrap">`--chunk-template=`</span>*PATHTEMPLATE*</span>  
Specify a template for the filenames in a <span class="nowrap">`chunkedhtml`</span> document. In the template, <span class="nowrap">`%n`</span> will be replaced by the chunk number (padded with leading 0s to 3 digits), <span class="nowrap">`%s`</span> with the section number of the chunk, <span class="nowrap">`%h`</span> with the heading text (with formatting removed), <span class="nowrap">`%i`</span> with the section identifier. For example, <span class="nowrap">`%section-%s-%i.html`</span> might be resolved to <span class="nowrap">`section-1.1-introduction.html`</span>. The characters <span class="nowrap">`/`</span> and <span class="nowrap">`\`</span> are not allowed in chunk templates and will be ignored. The default is <span class="nowrap">`%s-%i.html`</span>.

<span id="option--epub-chapter-level" class="option-anchor"><span class="nowrap">`--epub-chapter-level=`</span>*NUMBER*</span>  
*Deprecated synonym for <a href="#option--split-level" class="option"><span class="nowrap"><code>--split-level</code></span></a>.*

<span id="option--epub-cover-image" class="option-anchor"><span class="nowrap">`--epub-cover-image=`</span>*FILE*</span>  
Use the specified image as the EPUB cover. It is recommended that the image be less than 1000px in width and height. Note that in a Markdown source document you can also specify <span class="nowrap">`cover-image`</span> in a YAML metadata block (see [EPUB Metadata](#epub-metadata), below).

<span id="option--epub-title-page" class="option-anchor"><span class="nowrap">`--epub-title-page=true`</span>|<span class="nowrap">`false`</span></span>  
Determines whether a the title page is included in the EPUB (default is <span class="nowrap">`true`</span>).

<span id="option--epub-metadata" class="option-anchor"><span class="nowrap">`--epub-metadata=`</span>*FILE*</span>  
Look in the specified XML file for metadata for the EPUB. The file should contain a series of [Dublin Core elements](https://www.dublincore.org/specifications/dublin-core/dces/). For example:

     <dc:rights>Creative Commons</dc:rights>
     <dc:language>es-AR</dc:language>

By default, pandoc will include the following metadata elements: <span class="nowrap">`<dc:title>`</span> (from the document title), <span class="nowrap">`<dc:creator>`</span> (from the document authors), <span class="nowrap">`<dc:date>`</span> (from the document date, which should be in [ISO 8601 format](https://www.w3.org/TR/NOTE-datetime)), <span class="nowrap">`<dc:language>`</span> (from the <span class="nowrap">`lang`</span> variable, or, if is not set, the locale), and <span class="nowrap">`<dc:identifier id="BookId">`</span> (a randomly generated UUID). Any of these may be overridden by elements in the metadata file.

Note: if the source document is Markdown, a YAML metadata block in the document can be used instead. See below under [EPUB Metadata](#epub-metadata).

<span id="option--epub-embed-font" class="option-anchor"><span class="nowrap">`--epub-embed-font=`</span>*FILE*</span>  
Embed the specified font in the EPUB. This option can be repeated to embed multiple fonts. Wildcards can also be used: for example, <span class="nowrap">`DejaVuSans-*.ttf`</span>. However, if you use wildcards on the command line, be sure to escape them or put the whole filename in single quotes, to prevent them from being interpreted by the shell. To use the embedded fonts, you will need to add declarations like the following to your CSS (see <a href="#option--css" class="option"><span class="nowrap"><code>--css</code></span></a>):

    @font-face {
       font-family: DejaVuSans;
       font-style: normal;
       font-weight: normal;
       src:url("../fonts/DejaVuSans-Regular.ttf");
    }
    @font-face {
       font-family: DejaVuSans;
       font-style: normal;
       font-weight: bold;
       src:url("../fonts/DejaVuSans-Bold.ttf");
    }
    @font-face {
       font-family: DejaVuSans;
       font-style: italic;
       font-weight: normal;
       src:url("../fonts/DejaVuSans-Oblique.ttf");
    }
    @font-face {
       font-family: DejaVuSans;
       font-style: italic;
       font-weight: bold;
       src:url("../fonts/DejaVuSans-BoldOblique.ttf");
    }
    body { font-family: "DejaVuSans"; }

<span id="option--epub-subdirectory" class="option-anchor"><span class="nowrap">`--epub-subdirectory=`</span>*DIRNAME*</span>  
Specify the subdirectory in the OCF container that is to hold the EPUB-specific contents. The default is <span class="nowrap">`EPUB`</span>. To put the EPUB contents in the top level, use an empty string.

<span id="option--ipynb-output" class="option-anchor"><span class="nowrap">`--ipynb-output=all|none|best`</span></span>  
Determines how ipynb output cells are treated. <span class="nowrap">`all`</span> means that all of the data formats included in the original are preserved. <span class="nowrap">`none`</span> means that the contents of data cells are omitted. <span class="nowrap">`best`</span> causes pandoc to try to pick the richest data block in each output cell that is compatible with the output format. The default is <span class="nowrap">`best`</span>.

<span id="option--pdf-engine" class="option-anchor"><span class="nowrap">`--pdf-engine=`</span>*PROGRAM*</span>  
Use the specified engine when producing PDF output. Valid values are <span class="nowrap">`pdflatex`</span>, <span class="nowrap">`lualatex`</span>, <span class="nowrap">`xelatex`</span>, <span class="nowrap">`latexmk`</span>, <span class="nowrap">`tectonic`</span>, <span class="nowrap">`wkhtmltopdf`</span>, <span class="nowrap">`weasyprint`</span>, <span class="nowrap">`pagedjs-cli`</span>, <span class="nowrap">`prince`</span>, <span class="nowrap">`context`</span>, <span class="nowrap">`pdfroff`</span>, and <span class="nowrap">`typst`</span>. If the engine is not in your PATH, the full path of the engine may be specified here. If this option is not specified, pandoc uses the following defaults depending on the output format specified using <a href="#option--to" class="option"><span class="nowrap"><code>-t/--to</code></span></a>:

-   <a href="#option--to" class="option"><span class="nowrap"><code>-t latex</code></span></a> or none: <span class="nowrap">`pdflatex`</span> (other options: <span class="nowrap">`xelatex`</span>, <span class="nowrap">`lualatex`</span>, <span class="nowrap">`tectonic`</span>, <span class="nowrap">`latexmk`</span>)
-   <a href="#option--to" class="option"><span class="nowrap"><code>-t context</code></span></a>: <span class="nowrap">`context`</span>
-   <a href="#option--to" class="option"><span class="nowrap"><code>-t html</code></span></a>: <span class="nowrap">`weasyprint`</span> (other options: <span class="nowrap">`prince`</span>, <span class="nowrap">`wkhtmltopdf`</span>, <span class="nowrap">`pagedjs-cli`</span>; see [print-css.rocks](https://print-css.rocks) for a good introduction to PDF generation from HTML/CSS)
-   <a href="#option--to" class="option"><span class="nowrap"><code>-t ms</code></span></a>: <span class="nowrap">`pdfroff`</span>
-   <a href="#option--to" class="option"><span class="nowrap"><code>-t typst</code></span></a>: <span class="nowrap">`typst`</span>

<span id="option--pdf-engine-opt" class="option-anchor"><span class="nowrap">`--pdf-engine-opt=`</span>*STRING*</span>  
Use the given string as a command-line argument to the <span class="nowrap">`pdf-engine`</span>. For example, to use a persistent directory <span class="nowrap">`foo`</span> for <span class="nowrap">`latexmk`</span>’s auxiliary files, use <a href="#option--pdf-engine-opt" class="option"><span class="nowrap"><code>--pdf-engine-opt=-outdir=foo</code></span></a>. Note that no check for duplicate options is done.

<a href="#citation-rendering" class="anchor"></a>Citation rendering <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------------

<span id="option--citeproc" class="option-anchor"><span class="nowrap">`-C`</span>, <span class="nowrap">`--citeproc`</span></span>  
Process the citations in the file, replacing them with rendered citations and adding a bibliography. Citation processing will not take place unless bibliographic data is supplied, either through an external file specified using the <a href="#option--bibliography" class="option"><span class="nowrap"><code>--bibliography</code></span></a> option or the <span class="nowrap">`bibliography`</span> field in metadata, or via a <span class="nowrap">`references`</span> section in metadata containing a list of citations in CSL YAML format with Markdown formatting. The style is controlled by a [CSL](https://docs.citationstyles.org/en/stable/specification.html) stylesheet specified using the <a href="#option--csl" class="option"><span class="nowrap"><code>--csl</code></span></a> option or the <span class="nowrap">`csl`</span> field in metadata. (If no stylesheet is specified, the <span class="nowrap">`chicago-author-date`</span> style will be used by default.) The citation processing transformation may be applied before or after filters or Lua filters (see <a href="#option--filter" class="option"><span class="nowrap"><code>--filter</code></span></a>, <a href="#option--lua-filter" class="option"><span class="nowrap"><code>--lua-filter</code></span></a>): these transformations are applied in the order they appear on the command line. For more information, see the section on [Citations](#citations).

Note: if your target format is <span class="nowrap">`markdown`</span>, <span class="nowrap">`org`</span>, or <span class="nowrap">`typst`</span>, you will need to disable the <span class="nowrap">`citations`</span> extension (e.g., <a href="#option--to" class="option"><span class="nowrap"><code>-t markdown-citations</code></span></a>) to see the rendered citations and bibliography. Otherwise the format’s own citation syntax will be used.

<span id="option--bibliography" class="option-anchor"><span class="nowrap">`--bibliography=`</span>*FILE*</span>  
Set the <span class="nowrap">`bibliography`</span> field in the document’s metadata to *FILE*, overriding any value set in the metadata. If you supply this argument multiple times, each *FILE* will be added to bibliography. If *FILE* is a URL, it will be fetched via HTTP. If *FILE* is not found relative to the working directory, it will be sought in the resource path (see <a href="#option--resource-path" class="option"><span class="nowrap"><code>--resource-path</code></span></a>).

<span id="option--csl" class="option-anchor"><span class="nowrap">`--csl=`</span>*FILE*</span>  
Set the <span class="nowrap">`csl`</span> field in the document’s metadata to *FILE*, overriding any value set in the metadata. (This is equivalent to <a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata csl=FILE</code></span></a>.) If *FILE* is a URL, it will be fetched via HTTP. If *FILE* is not found relative to the working directory, it will be sought in the resource path (see <a href="#option--resource-path" class="option"><span class="nowrap"><code>--resource-path</code></span></a>) and finally in the <span class="nowrap">`csl`</span> subdirectory of the pandoc user data directory.

<span id="option--citation-abbreviations" class="option-anchor"><span class="nowrap">`--citation-abbreviations=`</span>*FILE*</span>  
Set the <span class="nowrap">`citation-abbreviations`</span> field in the document’s metadata to *FILE*, overriding any value set in the metadata. (This is equivalent to <a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata citation-abbreviations=FILE</code></span></a>.) If *FILE* is a URL, it will be fetched via HTTP. If *FILE* is not found relative to the working directory, it will be sought in the resource path (see <a href="#option--resource-path" class="option"><span class="nowrap"><code>--resource-path</code></span></a>) and finally in the <span class="nowrap">`csl`</span> subdirectory of the pandoc user data directory.

<span id="option--natbib" class="option-anchor"><span class="nowrap">`--natbib`</span></span>  
Use [<span class="nowrap">`natbib`</span>](https://ctan.org/pkg/natbib) for citations in LaTeX output. This option is not for use with the <a href="#option--citeproc" class="option"><span class="nowrap"><code>--citeproc</code></span></a> option or with PDF output. It is intended for use in producing a LaTeX file that can be processed with [<span class="nowrap">`bibtex`</span>](https://ctan.org/pkg/bibtex).

<span id="option--biblatex" class="option-anchor"><span class="nowrap">`--biblatex`</span></span>  
Use [<span class="nowrap">`biblatex`</span>](https://ctan.org/pkg/biblatex) for citations in LaTeX output. This option is not for use with the <a href="#option--citeproc" class="option"><span class="nowrap"><code>--citeproc</code></span></a> option or with PDF output. It is intended for use in producing a LaTeX file that can be processed with [<span class="nowrap">`bibtex`</span>](https://ctan.org/pkg/bibtex) or [<span class="nowrap">`biber`</span>](https://ctan.org/pkg/biber).

<a href="#math-rendering-in-html" class="anchor"></a>Math rendering in HTML <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------------------------

The default is to render TeX math as far as possible using Unicode characters. Formulas are put inside a <span class="nowrap">`span`</span> with <span class="nowrap">`class="math"`</span>, so that they may be styled differently from the surrounding text if needed. However, this gives acceptable results only for basic math, usually you will want to use <a href="#option--mathjax" class="option"><span class="nowrap"><code>--mathjax</code></span></a> or another of the following options.

<span id="option--mathjax" class="option-anchor"><span class="nowrap">`--mathjax`</span>\[<span class="nowrap">`=`</span>*URL*\]</span>  
Use [MathJax](https://www.mathjax.org) to display embedded TeX math in HTML output. TeX math will be put between <span class="nowrap">`\(...\)`</span> (for inline math) or <span class="nowrap">`\[...\]`</span> (for display math) and wrapped in <span class="nowrap">`<span>`</span> tags with class <span class="nowrap">`math`</span>. Then the MathJax JavaScript will render it. The *URL* should point to the <span class="nowrap">`MathJax.js`</span> load script. If a *URL* is not provided, a link to the Cloudflare CDN will be inserted.

<span id="option--mathml" class="option-anchor"><span class="nowrap">`--mathml`</span></span>  
Convert TeX math to [MathML](https://www.w3.org/Math/) (in <span class="nowrap">`epub3`</span>, <span class="nowrap">`docbook4`</span>, <span class="nowrap">`docbook5`</span>, <span class="nowrap">`jats`</span>, <span class="nowrap">`html4`</span> and <span class="nowrap">`html5`</span>). This is the default in <span class="nowrap">`odt`</span> output. MathML is supported natively by the main web browsers and select e-book readers.

<span id="option--webtex" class="option-anchor"><span class="nowrap">`--webtex`</span>\[<span class="nowrap">`=`</span>*URL*\]</span>  
Convert TeX formulas to <span class="nowrap">`<img>`</span> tags that link to an external script that converts formulas to images. The formula will be URL-encoded and concatenated with the URL provided. For SVG images you can for example use <a href="#option--webtex" class="option"><span class="nowrap"><code>--webtex https://latex.codecogs.com/svg.latex?</code></span></a>. If no URL is specified, the CodeCogs URL generating PNGs will be used (<span class="nowrap">`https://latex.codecogs.com/png.latex?`</span>). Note: the <a href="#option--webtex" class="option"><span class="nowrap"><code>--webtex</code></span></a> option will affect Markdown output as well as HTML, which is useful if you’re targeting a version of Markdown without native math support.

<span id="option--katex" class="option-anchor"><span class="nowrap">`--katex`</span>\[<span class="nowrap">`=`</span>*URL*\]</span>  
Use [KaTeX](https://github.com/Khan/KaTeX) to display embedded TeX math in HTML output. The *URL* is the base URL for the KaTeX library. That directory should contain a <span class="nowrap">`katex.min.js`</span> and a <span class="nowrap">`katex.min.css`</span> file. If a *URL* is not provided, a link to the KaTeX CDN will be inserted.

<span id="option--gladtex" class="option-anchor"><span class="nowrap">`--gladtex`</span></span>  
Enclose TeX math in <span class="nowrap">`<eq>`</span> tags in HTML output. The resulting HTML can then be processed by [GladTeX](https://humenda.github.io/GladTeX/) to produce SVG images of the typeset formulas and an HTML file with these images embedded.

    pandoc -s --gladtex input.md -o myfile.htex
    gladtex -d image_dir myfile.htex
    # produces myfile.html and images in image_dir

<a href="#options-for-wrapper-scripts" class="anchor"></a>Options for wrapper scripts <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------------------------------

<span id="option--dump-args[" class="option-anchor"><span class="nowrap">`--dump-args[=true|false]`</span></span>  
Print information about command-line arguments to *stdout*, then exit. This option is intended primarily for use in wrapper scripts. The first line of output contains the name of the output file specified with the <a href="#option--output" class="option"><span class="nowrap"><code>-o</code></span></a> option, or <span class="nowrap">`-`</span> (for *stdout*) if no output file was specified. The remaining lines contain the command-line arguments, one per line, in the order they appear. These do not include regular pandoc options and their arguments, but do include any options appearing after a <span class="nowrap">`--`</span> separator at the end of the line.

<span id="option--ignore-args[" class="option-anchor"><span class="nowrap">`--ignore-args[=true|false]`</span></span>  
Ignore command-line arguments (for use in wrapper scripts). Regular pandoc options are not ignored. Thus, for example,

    pandoc --ignore-args -o foo.html -s foo.txt -- -e latin1

is equivalent to

    pandoc -o foo.html -s

<a href="#exit-codes" class="anchor"></a>Exit codes <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
===================================================================================================================

If pandoc completes successfully, it will return exit code 0. Nonzero exit codes have the following meanings:

<table><thead><tr class="header"><th style="text-align: right;">Code</th><th style="text-align: left;">Error</th></tr></thead><tbody><tr class="odd"><td style="text-align: right;">1</td><td style="text-align: left;">PandocIOError</td></tr><tr class="even"><td style="text-align: right;">3</td><td style="text-align: left;">PandocFailOnWarningError</td></tr><tr class="odd"><td style="text-align: right;">4</td><td style="text-align: left;">PandocAppError</td></tr><tr class="even"><td style="text-align: right;">5</td><td style="text-align: left;">PandocTemplateError</td></tr><tr class="odd"><td style="text-align: right;">6</td><td style="text-align: left;">PandocOptionError</td></tr><tr class="even"><td style="text-align: right;">21</td><td style="text-align: left;">PandocUnknownReaderError</td></tr><tr class="odd"><td style="text-align: right;">22</td><td style="text-align: left;">PandocUnknownWriterError</td></tr><tr class="even"><td style="text-align: right;">23</td><td style="text-align: left;">PandocUnsupportedExtensionError</td></tr><tr class="odd"><td style="text-align: right;">24</td><td style="text-align: left;">PandocCiteprocError</td></tr><tr class="even"><td style="text-align: right;">25</td><td style="text-align: left;">PandocBibliographyError</td></tr><tr class="odd"><td style="text-align: right;">31</td><td style="text-align: left;">PandocEpubSubdirectoryError</td></tr><tr class="even"><td style="text-align: right;">43</td><td style="text-align: left;">PandocPDFError</td></tr><tr class="odd"><td style="text-align: right;">44</td><td style="text-align: left;">PandocXMLError</td></tr><tr class="even"><td style="text-align: right;">47</td><td style="text-align: left;">PandocPDFProgramNotFoundError</td></tr><tr class="odd"><td style="text-align: right;">61</td><td style="text-align: left;">PandocHttpError</td></tr><tr class="even"><td style="text-align: right;">62</td><td style="text-align: left;">PandocShouldNeverHappenError</td></tr><tr class="odd"><td style="text-align: right;">63</td><td style="text-align: left;">PandocSomeError</td></tr><tr class="even"><td style="text-align: right;">64</td><td style="text-align: left;">PandocParseError</td></tr><tr class="odd"><td style="text-align: right;">66</td><td style="text-align: left;">PandocMakePDFError</td></tr><tr class="even"><td style="text-align: right;">67</td><td style="text-align: left;">PandocSyntaxMapError</td></tr><tr class="odd"><td style="text-align: right;">83</td><td style="text-align: left;">PandocFilterError</td></tr><tr class="even"><td style="text-align: right;">84</td><td style="text-align: left;">PandocLuaError</td></tr><tr class="odd"><td style="text-align: right;">89</td><td style="text-align: left;">PandocNoScriptingEngine</td></tr><tr class="even"><td style="text-align: right;">91</td><td style="text-align: left;">PandocMacroLoop</td></tr><tr class="odd"><td style="text-align: right;">92</td><td style="text-align: left;">PandocUTF8DecodingError</td></tr><tr class="even"><td style="text-align: right;">93</td><td style="text-align: left;">PandocIpynbDecodingError</td></tr><tr class="odd"><td style="text-align: right;">94</td><td style="text-align: left;">PandocUnsupportedCharsetError</td></tr><tr class="even"><td style="text-align: right;">97</td><td style="text-align: left;">PandocCouldNotFindDataFileError</td></tr><tr class="odd"><td style="text-align: right;">98</td><td style="text-align: left;">PandocCouldNotFindMetadataFileError</td></tr><tr class="even"><td style="text-align: right;">99</td><td style="text-align: left;">PandocResourceNotFound</td></tr></tbody></table>

<a href="#defaults-files" class="anchor"></a>Defaults files <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
===========================================================================================================================

The <a href="#option--defaults" class="option"><span class="nowrap"><code>--defaults</code></span></a> option may be used to specify a package of options, in the form of a YAML file.

Fields that are omitted will just have their regular default values. So a defaults file can be as simple as one line:

    verbosity: INFO

In fields that expect a file path (or list of file paths), the following syntax may be used to interpolate environment variables:

    csl:  ${HOME}/mycsldir/special.csl

<span class="nowrap">`${USERDATA}`</span> may also be used; this will always resolve to the user data directory that is current when the defaults file is parsed, regardless of the setting of the environment variable <span class="nowrap">`USERDATA`</span>.

<span class="nowrap">`${.}`</span> will resolve to the directory containing the defaults file itself. This allows you to refer to resources contained in that directory:

    epub-cover-image: ${.}/cover.jpg
    epub-metadata: ${.}/meta.xml
    resource-path:
    - .             # the working directory from which pandoc is run
    - ${.}/images   # the images subdirectory of the directory
                    # containing this defaults file

This environment variable interpolation syntax *only* works in fields that expect file paths.

Defaults files can be placed in the <span class="nowrap">`defaults`</span> subdirectory of the user data directory and used from any directory. For example, one could create a file specifying defaults for writing letters, save it as <span class="nowrap">`letter.yaml`</span> in the <span class="nowrap">`defaults`</span> subdirectory of the user data directory, and then invoke these defaults from any directory using <span class="nowrap">`pandoc --defaults letter`</span> or <span class="nowrap">`pandoc -dletter`</span>.

When multiple defaults are used, their contents will be combined.

Note that, where command-line arguments may be repeated (<a href="#option--metadata-file" class="option"><span class="nowrap"><code>--metadata-file</code></span></a>, <a href="#option--css" class="option"><span class="nowrap"><code>--css</code></span></a>, <a href="#option--include-in-header" class="option"><span class="nowrap"><code>--include-in-header</code></span></a>, <a href="#option--include-before-body" class="option"><span class="nowrap"><code>--include-before-body</code></span></a>, <a href="#option--include-after-body" class="option"><span class="nowrap"><code>--include-after-body</code></span></a>, <a href="#option--variable" class="option"><span class="nowrap"><code>--variable</code></span></a>, <a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata</code></span></a>, <a href="#option--syntax-definition" class="option"><span class="nowrap"><code>--syntax-definition</code></span></a>), the values specified on the command line will combine with values specified in the defaults file, rather than replacing them.

The following tables show the mapping between the command line and defaults file entries.

<table style="width:98%;"><colgroup><col style="width: 48%" /><col style="width: 50%" /></colgroup><thead><tr class="header"><th style="text-align: left;">command line</th><th style="text-align: left;">defaults file</th></tr></thead><tbody><tr class="odd"><td style="text-align: left;"><pre><code>foo.md</code></pre></td><td style="text-align: left;"><div id="cb22" class="sourceCode"><div class="sourceCode" id="cb2"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb2-1"><a href="#cb2-1" aria-hidden="true"></a><span class="fu">input-file</span><span class="kw">:</span><span class="at"> foo.md</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>foo.md bar.md
</code></pre></td><td style="text-align: left;"><div id="cb24" class="sourceCode"><div class="sourceCode" id="cb4"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb4-1"><a href="#cb4-1" aria-hidden="true"></a><span class="fu">input-files</span><span class="kw">:</span></span>
<span id="cb4-2"><a href="#cb4-2" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> foo.md</span></span>
<span id="cb4-3"><a href="#cb4-3" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> bar.md</span></span></code></pre></div></div></td></tr></tbody></table>

The value of <span class="nowrap">`input-files`</span> may be left empty to indicate input from stdin, and it can be an empty sequence <span class="nowrap">`[]`</span> for no input.

<a href="#general-options-1" class="anchor"></a>General options <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------------

<table style="width:98%;"><colgroup><col style="width: 48%" /><col style="width: 50%" /></colgroup><thead><tr class="header"><th style="text-align: left;">command line</th><th style="text-align: left;">defaults file</th></tr></thead><tbody><tr class="odd"><td style="text-align: left;"><pre><code>--from markdown+emoji</code></pre></td><td style="text-align: left;"><div id="cb26" class="sourceCode"><div class="sourceCode" id="cb2"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb2-1"><a href="#cb2-1" aria-hidden="true"></a><span class="fu">from</span><span class="kw">:</span><span class="at"> markdown+emoji</span></span></code></pre></div></div><div id="cb27" class="sourceCode"><div class="sourceCode" id="cb3"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb3-1"><a href="#cb3-1" aria-hidden="true"></a><span class="fu">reader</span><span class="kw">:</span><span class="at"> markdown+emoji</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--to markdown+hard_line_breaks</code></pre></td><td style="text-align: left;"><div id="cb29" class="sourceCode"><div class="sourceCode" id="cb5"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb5-1"><a href="#cb5-1" aria-hidden="true"></a><span class="fu">to</span><span class="kw">:</span><span class="at"> markdown+hard_line_breaks</span></span></code></pre></div></div><div id="cb30" class="sourceCode"><div class="sourceCode" id="cb6"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb6-1"><a href="#cb6-1" aria-hidden="true"></a><span class="fu">writer</span><span class="kw">:</span><span class="at"> markdown+hard_line_breaks</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--output foo.pdf</code></pre></td><td style="text-align: left;"><div id="cb32" class="sourceCode"><div class="sourceCode" id="cb8"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb8-1"><a href="#cb8-1" aria-hidden="true"></a><span class="fu">output-file</span><span class="kw">:</span><span class="at"> foo.pdf</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--output -</code></pre></td><td style="text-align: left;"><div id="cb34" class="sourceCode"><div class="sourceCode" id="cb10"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb10-1"><a href="#cb10-1" aria-hidden="true"></a><span class="fu">output-file</span><span class="kw">:</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--data-dir dir</code></pre></td><td style="text-align: left;"><div id="cb36" class="sourceCode"><div class="sourceCode" id="cb12"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb12-1"><a href="#cb12-1" aria-hidden="true"></a><span class="fu">data-dir</span><span class="kw">:</span><span class="at"> dir</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--defaults file</code></pre></td><td style="text-align: left;"><div id="cb38" class="sourceCode"><div class="sourceCode" id="cb14"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb14-1"><a href="#cb14-1" aria-hidden="true"></a><span class="fu">defaults</span><span class="kw">:</span></span>
<span id="cb14-2"><a href="#cb14-2" aria-hidden="true"></a><span class="kw">-</span><span class="at"> file</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--verbose</code></pre></td><td style="text-align: left;"><div id="cb40" class="sourceCode"><div class="sourceCode" id="cb16"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb16-1"><a href="#cb16-1" aria-hidden="true"></a><span class="fu">verbosity</span><span class="kw">:</span><span class="at"> INFO</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--quiet</code></pre></td><td style="text-align: left;"><div id="cb42" class="sourceCode"><div class="sourceCode" id="cb18"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb18-1"><a href="#cb18-1" aria-hidden="true"></a><span class="fu">verbosity</span><span class="kw">:</span><span class="at"> ERROR</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--fail-if-warnings</code></pre></td><td style="text-align: left;"><div id="cb44" class="sourceCode"><div class="sourceCode" id="cb20"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb20-1"><a href="#cb20-1" aria-hidden="true"></a><span class="fu">fail-if-warnings</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--sandbox</code></pre></td><td style="text-align: left;"><div id="cb46" class="sourceCode"><div class="sourceCode" id="cb22"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb22-1"><a href="#cb22-1" aria-hidden="true"></a><span class="fu">sandbox</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--log=FILE</code></pre></td><td style="text-align: left;"><div id="cb48" class="sourceCode"><div class="sourceCode" id="cb24"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb24-1"><a href="#cb24-1" aria-hidden="true"></a><span class="fu">log-file</span><span class="kw">:</span><span class="at"> FILE</span></span></code></pre></div></div></td></tr></tbody></table>

Options specified in a defaults file itself always have priority over those in another file included with a <span class="nowrap">`defaults:`</span> entry.

<span class="nowrap">`verbosity`</span> can have the values <span class="nowrap">`ERROR`</span>, <span class="nowrap">`WARNING`</span>, or <span class="nowrap">`INFO`</span>.

<a href="#reader-options-1" class="anchor"></a>Reader options <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------

<table style="width:98%;"><colgroup><col style="width: 48%" /><col style="width: 50%" /></colgroup><thead><tr class="header"><th style="text-align: left;">command line</th><th style="text-align: left;">defaults file</th></tr></thead><tbody><tr class="odd"><td style="text-align: left;"><pre><code>--shift-heading-level-by -1</code></pre></td><td style="text-align: left;"><div id="cb50" class="sourceCode"><div class="sourceCode" id="cb2"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb2-1"><a href="#cb2-1" aria-hidden="true"></a><span class="fu">shift-heading-level-by</span><span class="kw">:</span><span class="at"> </span><span class="dv">-1</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--indented-code-classes python</code></pre></td><td style="text-align: left;"><div id="cb52" class="sourceCode"><div class="sourceCode" id="cb4"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb4-1"><a href="#cb4-1" aria-hidden="true"></a><span class="fu">indented-code-classes</span><span class="kw">:</span></span>
<span id="cb4-2"><a href="#cb4-2" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> python</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--default-image-extension &quot;.jpg&quot;</code></pre></td><td style="text-align: left;"><div id="cb54" class="sourceCode"><div class="sourceCode" id="cb6"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb6-1"><a href="#cb6-1" aria-hidden="true"></a><span class="fu">default-image-extension</span><span class="kw">:</span><span class="at"> </span><span class="st">&#39;.jpg&#39;</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--file-scope</code></pre></td><td style="text-align: left;"><div id="cb56" class="sourceCode"><div class="sourceCode" id="cb8"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb8-1"><a href="#cb8-1" aria-hidden="true"></a><span class="fu">file-scope</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--citeproc \
 --lua-filter count-words.lua \
 --filter special.lua
</code></pre></td><td style="text-align: left;"><div id="cb58" class="sourceCode"><div class="sourceCode" id="cb10"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb10-1"><a href="#cb10-1" aria-hidden="true"></a><span class="fu">filters</span><span class="kw">:</span></span>
<span id="cb10-2"><a href="#cb10-2" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> citeproc</span></span>
<span id="cb10-3"><a href="#cb10-3" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> count-words.lua</span></span>
<span id="cb10-4"><a href="#cb10-4" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> </span><span class="fu">type</span><span class="kw">:</span><span class="at"> json</span></span>
<span id="cb10-5"><a href="#cb10-5" aria-hidden="true"></a><span class="at">    </span><span class="fu">path</span><span class="kw">:</span><span class="at"> special.lua</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--metadata key=value \
 --metadata key2</code></pre></td><td style="text-align: left;"><div id="cb60" class="sourceCode"><div class="sourceCode" id="cb12"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb12-1"><a href="#cb12-1" aria-hidden="true"></a><span class="fu">metadata</span><span class="kw">:</span></span>
<span id="cb12-2"><a href="#cb12-2" aria-hidden="true"></a><span class="at">  </span><span class="fu">key</span><span class="kw">:</span><span class="at"> value</span></span>
<span id="cb12-3"><a href="#cb12-3" aria-hidden="true"></a><span class="at">  </span><span class="fu">key2</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--metadata-file meta.yaml</code></pre></td><td style="text-align: left;"><div id="cb62" class="sourceCode"><div class="sourceCode" id="cb14"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb14-1"><a href="#cb14-1" aria-hidden="true"></a><span class="fu">metadata-files</span><span class="kw">:</span></span>
<span id="cb14-2"><a href="#cb14-2" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> meta.yaml</span></span></code></pre></div></div><div id="cb63" class="sourceCode"><div class="sourceCode" id="cb15"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb15-1"><a href="#cb15-1" aria-hidden="true"></a><span class="fu">metadata-file</span><span class="kw">:</span><span class="at"> meta.yaml</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--preserve-tabs</code></pre></td><td style="text-align: left;"><div id="cb65" class="sourceCode"><div class="sourceCode" id="cb17"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb17-1"><a href="#cb17-1" aria-hidden="true"></a><span class="fu">preserve-tabs</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--tab-stop 8</code></pre></td><td style="text-align: left;"><div id="cb67" class="sourceCode"><div class="sourceCode" id="cb19"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb19-1"><a href="#cb19-1" aria-hidden="true"></a><span class="fu">tab-stop</span><span class="kw">:</span><span class="at"> </span><span class="dv">8</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--track-changes accept</code></pre></td><td style="text-align: left;"><div id="cb69" class="sourceCode"><div class="sourceCode" id="cb21"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb21-1"><a href="#cb21-1" aria-hidden="true"></a><span class="fu">track-changes</span><span class="kw">:</span><span class="at"> accept</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--extract-media dir</code></pre></td><td style="text-align: left;"><div id="cb71" class="sourceCode"><div class="sourceCode" id="cb23"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb23-1"><a href="#cb23-1" aria-hidden="true"></a><span class="fu">extract-media</span><span class="kw">:</span><span class="at"> dir</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--abbreviations abbrevs.txt</code></pre></td><td style="text-align: left;"><div id="cb73" class="sourceCode"><div class="sourceCode" id="cb25"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb25-1"><a href="#cb25-1" aria-hidden="true"></a><span class="fu">abbreviations</span><span class="kw">:</span><span class="at"> abbrevs.txt</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--trace</code></pre></td><td style="text-align: left;"><div id="cb75" class="sourceCode"><div class="sourceCode" id="cb27"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb27-1"><a href="#cb27-1" aria-hidden="true"></a><span class="fu">trace</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr></tbody></table>

Metadata values specified in a defaults file are parsed as literal string text, not Markdown.

Filters will be assumed to be Lua filters if they have the <span class="nowrap">`.lua`</span> extension, and JSON filters otherwise. But the filter type can also be specified explicitly, as shown. Filters are run in the order specified. To include the built-in citeproc filter, use either <span class="nowrap">`citeproc`</span> or <span class="nowrap">`{type: citeproc}`</span>.

<a href="#general-writer-options-1" class="anchor"></a>General writer options <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------------------------

<table style="width:98%;"><colgroup><col style="width: 48%" /><col style="width: 50%" /></colgroup><thead><tr class="header"><th style="text-align: left;">command line</th><th style="text-align: left;">defaults file</th></tr></thead><tbody><tr class="odd"><td style="text-align: left;"><pre><code>--standalone</code></pre></td><td style="text-align: left;"><div id="cb77" class="sourceCode"><div class="sourceCode" id="cb2"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb2-1"><a href="#cb2-1" aria-hidden="true"></a><span class="fu">standalone</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--template letter</code></pre></td><td style="text-align: left;"><div id="cb79" class="sourceCode"><div class="sourceCode" id="cb4"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb4-1"><a href="#cb4-1" aria-hidden="true"></a><span class="fu">template</span><span class="kw">:</span><span class="at"> letter</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--variable key=val \
  --variable key2</code></pre></td><td style="text-align: left;"><div id="cb81" class="sourceCode"><div class="sourceCode" id="cb6"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb6-1"><a href="#cb6-1" aria-hidden="true"></a><span class="fu">variables</span><span class="kw">:</span></span>
<span id="cb6-2"><a href="#cb6-2" aria-hidden="true"></a><span class="at">  </span><span class="fu">key</span><span class="kw">:</span><span class="at"> val</span></span>
<span id="cb6-3"><a href="#cb6-3" aria-hidden="true"></a><span class="at">  </span><span class="fu">key2</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--eol nl</code></pre></td><td style="text-align: left;"><div id="cb83" class="sourceCode"><div class="sourceCode" id="cb8"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb8-1"><a href="#cb8-1" aria-hidden="true"></a><span class="fu">eol</span><span class="kw">:</span><span class="at"> nl</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--dpi 300</code></pre></td><td style="text-align: left;"><div id="cb85" class="sourceCode"><div class="sourceCode" id="cb10"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb10-1"><a href="#cb10-1" aria-hidden="true"></a><span class="fu">dpi</span><span class="kw">:</span><span class="at"> </span><span class="dv">300</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--wrap 60</code></pre></td><td style="text-align: left;"><div id="cb87" class="sourceCode"><div class="sourceCode" id="cb12"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb12-1"><a href="#cb12-1" aria-hidden="true"></a><span class="fu">wrap</span><span class="kw">:</span><span class="at"> </span><span class="dv">60</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--columns 72</code></pre></td><td style="text-align: left;"><div id="cb89" class="sourceCode"><div class="sourceCode" id="cb14"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb14-1"><a href="#cb14-1" aria-hidden="true"></a><span class="fu">columns</span><span class="kw">:</span><span class="at"> </span><span class="dv">72</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--table-of-contents</code></pre></td><td style="text-align: left;"><div id="cb91" class="sourceCode"><div class="sourceCode" id="cb16"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb16-1"><a href="#cb16-1" aria-hidden="true"></a><span class="fu">table-of-contents</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--toc</code></pre></td><td style="text-align: left;"><div id="cb93" class="sourceCode"><div class="sourceCode" id="cb18"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb18-1"><a href="#cb18-1" aria-hidden="true"></a><span class="fu">toc</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--toc-depth 3</code></pre></td><td style="text-align: left;"><div id="cb95" class="sourceCode"><div class="sourceCode" id="cb20"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb20-1"><a href="#cb20-1" aria-hidden="true"></a><span class="fu">toc-depth</span><span class="kw">:</span><span class="at"> </span><span class="dv">3</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--strip-comments</code></pre></td><td style="text-align: left;"><div id="cb97" class="sourceCode"><div class="sourceCode" id="cb22"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb22-1"><a href="#cb22-1" aria-hidden="true"></a><span class="fu">strip-comments</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--no-highlight</code></pre></td><td style="text-align: left;"><div id="cb99" class="sourceCode"><div class="sourceCode" id="cb24"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb24-1"><a href="#cb24-1" aria-hidden="true"></a><span class="fu">highlight-style</span><span class="kw">:</span><span class="at"> </span><span class="ch">null</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--highlight-style kate</code></pre></td><td style="text-align: left;"><div id="cb101" class="sourceCode"><div class="sourceCode" id="cb26"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb26-1"><a href="#cb26-1" aria-hidden="true"></a><span class="fu">highlight-style</span><span class="kw">:</span><span class="at"> kate</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--syntax-definition mylang.xml</code></pre></td><td style="text-align: left;"><div id="cb103" class="sourceCode"><div class="sourceCode" id="cb28"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb28-1"><a href="#cb28-1" aria-hidden="true"></a><span class="fu">syntax-definitions</span><span class="kw">:</span></span>
<span id="cb28-2"><a href="#cb28-2" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> mylang.xml</span></span></code></pre></div></div><div id="cb104" class="sourceCode"><div class="sourceCode" id="cb29"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb29-1"><a href="#cb29-1" aria-hidden="true"></a><span class="fu">syntax-definition</span><span class="kw">:</span><span class="at"> mylang.xml</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--include-in-header inc.tex</code></pre></td><td style="text-align: left;"><div id="cb106" class="sourceCode"><div class="sourceCode" id="cb31"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb31-1"><a href="#cb31-1" aria-hidden="true"></a><span class="fu">include-in-header</span><span class="kw">:</span></span>
<span id="cb31-2"><a href="#cb31-2" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> inc.tex</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--include-before-body inc.tex</code></pre></td><td style="text-align: left;"><div id="cb108" class="sourceCode"><div class="sourceCode" id="cb33"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb33-1"><a href="#cb33-1" aria-hidden="true"></a><span class="fu">include-before-body</span><span class="kw">:</span></span>
<span id="cb33-2"><a href="#cb33-2" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> inc.tex</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--include-after-body inc.tex</code></pre></td><td style="text-align: left;"><div id="cb110" class="sourceCode"><div class="sourceCode" id="cb35"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb35-1"><a href="#cb35-1" aria-hidden="true"></a><span class="fu">include-after-body</span><span class="kw">:</span></span>
<span id="cb35-2"><a href="#cb35-2" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> inc.tex</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--resource-path .:foo</code></pre></td><td style="text-align: left;"><div id="cb112" class="sourceCode"><div class="sourceCode" id="cb37"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb37-1"><a href="#cb37-1" aria-hidden="true"></a><span class="fu">resource-path</span><span class="kw">:</span><span class="at"> </span><span class="kw">[</span><span class="st">&#39;.&#39;</span><span class="kw">,</span><span class="st">&#39;foo&#39;</span><span class="kw">]</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--request-header foo:bar</code></pre></td><td style="text-align: left;"><div id="cb114" class="sourceCode"><div class="sourceCode" id="cb39"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb39-1"><a href="#cb39-1" aria-hidden="true"></a><span class="fu">request-headers</span><span class="kw">:</span></span>
<span id="cb39-2"><a href="#cb39-2" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> </span><span class="kw">[</span><span class="st">&quot;User-Agent&quot;</span><span class="kw">,</span><span class="at"> </span><span class="st">&quot;Mozilla/5.0&quot;</span><span class="kw">]</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--no-check-certificate</code></pre></td><td style="text-align: left;"><div id="cb116" class="sourceCode"><div class="sourceCode" id="cb41"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb41-1"><a href="#cb41-1" aria-hidden="true"></a><span class="fu">no-check-certificate</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr></tbody></table>

<a href="#options-affecting-specific-writers-1" class="anchor"></a>Options affecting specific writers <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------------------------------------------------

<table style="width:98%;"><colgroup><col style="width: 48%" /><col style="width: 50%" /></colgroup><thead><tr class="header"><th style="text-align: left;">command line</th><th style="text-align: left;">defaults file</th></tr></thead><tbody><tr class="odd"><td style="text-align: left;"><pre><code>--self-contained</code></pre></td><td style="text-align: left;"><div id="cb118" class="sourceCode"><div class="sourceCode" id="cb2"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb2-1"><a href="#cb2-1" aria-hidden="true"></a><span class="fu">self-contained</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--link-images</code></pre></td><td style="text-align: left;"><div id="cb120" class="sourceCode"><div class="sourceCode" id="cb4"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb4-1"><a href="#cb4-1" aria-hidden="true"></a><span class="fu">link-images</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--html-q-tags</code></pre></td><td style="text-align: left;"><div id="cb122" class="sourceCode"><div class="sourceCode" id="cb6"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb6-1"><a href="#cb6-1" aria-hidden="true"></a><span class="fu">html-q-tags</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--ascii</code></pre></td><td style="text-align: left;"><div id="cb124" class="sourceCode"><div class="sourceCode" id="cb8"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb8-1"><a href="#cb8-1" aria-hidden="true"></a><span class="fu">ascii</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--reference-links</code></pre></td><td style="text-align: left;"><div id="cb126" class="sourceCode"><div class="sourceCode" id="cb10"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb10-1"><a href="#cb10-1" aria-hidden="true"></a><span class="fu">reference-links</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--reference-location block</code></pre></td><td style="text-align: left;"><div id="cb128" class="sourceCode"><div class="sourceCode" id="cb12"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb12-1"><a href="#cb12-1" aria-hidden="true"></a><span class="fu">reference-location</span><span class="kw">:</span><span class="at"> block</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--figure-caption-position=above</code></pre></td><td style="text-align: left;"><div id="cb130" class="sourceCode"><div class="sourceCode" id="cb14"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb14-1"><a href="#cb14-1" aria-hidden="true"></a><span class="fu">figure-caption-position</span><span class="kw">:</span><span class="at"> above</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--table-caption-position=below</code></pre></td><td style="text-align: left;"><div id="cb132" class="sourceCode"><div class="sourceCode" id="cb16"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb16-1"><a href="#cb16-1" aria-hidden="true"></a><span class="fu">table-caption-position</span><span class="kw">:</span><span class="at"> below</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--markdown-headings atx</code></pre></td><td style="text-align: left;"><div id="cb134" class="sourceCode"><div class="sourceCode" id="cb18"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb18-1"><a href="#cb18-1" aria-hidden="true"></a><span class="fu">markdown-headings</span><span class="kw">:</span><span class="at"> atx</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--list-tables</code></pre></td><td style="text-align: left;"><div id="cb136" class="sourceCode"><div class="sourceCode" id="cb20"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb20-1"><a href="#cb20-1" aria-hidden="true"></a><span class="fu">list-tables</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--top-level-division chapter</code></pre></td><td style="text-align: left;"><div id="cb138" class="sourceCode"><div class="sourceCode" id="cb22"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb22-1"><a href="#cb22-1" aria-hidden="true"></a><span class="fu">top-level-division</span><span class="kw">:</span><span class="at"> chapter</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--number-sections</code></pre></td><td style="text-align: left;"><div id="cb140" class="sourceCode"><div class="sourceCode" id="cb24"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb24-1"><a href="#cb24-1" aria-hidden="true"></a><span class="fu">number-sections</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--number-offset=1,4</code></pre></td><td style="text-align: left;"><div id="cb142" class="sourceCode"><div class="sourceCode" id="cb26"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb26-1"><a href="#cb26-1" aria-hidden="true"></a><span class="fu">number-offset</span><span class="kw">:</span><span class="at"> \[1,4\]</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--listings</code></pre></td><td style="text-align: left;"><div id="cb144" class="sourceCode"><div class="sourceCode" id="cb28"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb28-1"><a href="#cb28-1" aria-hidden="true"></a><span class="fu">listings</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--list-of-figures</code></pre></td><td style="text-align: left;"><div id="cb146" class="sourceCode"><div class="sourceCode" id="cb30"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb30-1"><a href="#cb30-1" aria-hidden="true"></a><span class="fu">list-of-figures</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--lof</code></pre></td><td style="text-align: left;"><div id="cb148" class="sourceCode"><div class="sourceCode" id="cb32"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb32-1"><a href="#cb32-1" aria-hidden="true"></a><span class="fu">lof</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--list-of-tables</code></pre></td><td style="text-align: left;"><div id="cb150" class="sourceCode"><div class="sourceCode" id="cb34"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb34-1"><a href="#cb34-1" aria-hidden="true"></a><span class="fu">list-of-tables</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--lot</code></pre></td><td style="text-align: left;"><div id="cb152" class="sourceCode"><div class="sourceCode" id="cb36"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb36-1"><a href="#cb36-1" aria-hidden="true"></a><span class="fu">lot</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--incremental</code></pre></td><td style="text-align: left;"><div id="cb154" class="sourceCode"><div class="sourceCode" id="cb38"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb38-1"><a href="#cb38-1" aria-hidden="true"></a><span class="fu">incremental</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--slide-level 2</code></pre></td><td style="text-align: left;"><div id="cb156" class="sourceCode"><div class="sourceCode" id="cb40"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb40-1"><a href="#cb40-1" aria-hidden="true"></a><span class="fu">slide-level</span><span class="kw">:</span><span class="at"> </span><span class="dv">2</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--section-divs</code></pre></td><td style="text-align: left;"><div id="cb158" class="sourceCode"><div class="sourceCode" id="cb42"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb42-1"><a href="#cb42-1" aria-hidden="true"></a><span class="fu">section-divs</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--email-obfuscation references</code></pre></td><td style="text-align: left;"><div id="cb160" class="sourceCode"><div class="sourceCode" id="cb44"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb44-1"><a href="#cb44-1" aria-hidden="true"></a><span class="fu">email-obfuscation</span><span class="kw">:</span><span class="at"> references</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--id-prefix ch1</code></pre></td><td style="text-align: left;"><div id="cb162" class="sourceCode"><div class="sourceCode" id="cb46"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb46-1"><a href="#cb46-1" aria-hidden="true"></a><span class="fu">identifier-prefix</span><span class="kw">:</span><span class="at"> ch1</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--title-prefix MySite</code></pre></td><td style="text-align: left;"><div id="cb164" class="sourceCode"><div class="sourceCode" id="cb48"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb48-1"><a href="#cb48-1" aria-hidden="true"></a><span class="fu">title-prefix</span><span class="kw">:</span><span class="at"> MySite</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--css styles/screen.css  \
  --css styles/special.css</code></pre></td><td style="text-align: left;"><div id="cb166" class="sourceCode"><div class="sourceCode" id="cb50"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb50-1"><a href="#cb50-1" aria-hidden="true"></a><span class="fu">css</span><span class="kw">:</span></span>
<span id="cb50-2"><a href="#cb50-2" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> styles/screen.css</span></span>
<span id="cb50-3"><a href="#cb50-3" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> styles/special.css</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--reference-doc my.docx</code></pre></td><td style="text-align: left;"><div id="cb168" class="sourceCode"><div class="sourceCode" id="cb52"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb52-1"><a href="#cb52-1" aria-hidden="true"></a><span class="fu">reference-doc</span><span class="kw">:</span><span class="at"> my.docx</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--epub-cover-image cover.jpg</code></pre></td><td style="text-align: left;"><div id="cb170" class="sourceCode"><div class="sourceCode" id="cb54"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb54-1"><a href="#cb54-1" aria-hidden="true"></a><span class="fu">epub-cover-image</span><span class="kw">:</span><span class="at"> cover.jpg</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--epub-title-page=false</code></pre></td><td style="text-align: left;"><div id="cb172" class="sourceCode"><div class="sourceCode" id="cb56"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb56-1"><a href="#cb56-1" aria-hidden="true"></a><span class="fu">epub-title-page</span><span class="kw">:</span><span class="at"> </span><span class="ch">false</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--epub-metadata meta.xml</code></pre></td><td style="text-align: left;"><div id="cb174" class="sourceCode"><div class="sourceCode" id="cb58"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb58-1"><a href="#cb58-1" aria-hidden="true"></a><span class="fu">epub-metadata</span><span class="kw">:</span><span class="at"> meta.xml</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--epub-embed-font special.otf \
  --epub-embed-font headline.otf</code></pre></td><td style="text-align: left;"><div id="cb176" class="sourceCode"><div class="sourceCode" id="cb60"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb60-1"><a href="#cb60-1" aria-hidden="true"></a><span class="fu">epub-fonts</span><span class="kw">:</span></span>
<span id="cb60-2"><a href="#cb60-2" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> special.otf</span></span>
<span id="cb60-3"><a href="#cb60-3" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> headline.otf</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--split-level 2</code></pre></td><td style="text-align: left;"><div id="cb178" class="sourceCode"><div class="sourceCode" id="cb62"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb62-1"><a href="#cb62-1" aria-hidden="true"></a><span class="fu">split-level</span><span class="kw">:</span><span class="at"> </span><span class="dv">2</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--chunk-template=&quot;%i.html&quot;</code></pre></td><td style="text-align: left;"><div id="cb180" class="sourceCode"><div class="sourceCode" id="cb64"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb64-1"><a href="#cb64-1" aria-hidden="true"></a><span class="fu">chunk-template</span><span class="kw">:</span><span class="at"> </span><span class="st">&quot;%i.html&quot;</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--epub-subdirectory=&quot;&quot;</code></pre></td><td style="text-align: left;"><div id="cb182" class="sourceCode"><div class="sourceCode" id="cb66"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb66-1"><a href="#cb66-1" aria-hidden="true"></a><span class="fu">epub-subdirectory</span><span class="kw">:</span><span class="at"> </span><span class="st">&#39;&#39;</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--ipynb-output best</code></pre></td><td style="text-align: left;"><div id="cb184" class="sourceCode"><div class="sourceCode" id="cb68"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb68-1"><a href="#cb68-1" aria-hidden="true"></a><span class="fu">ipynb-output</span><span class="kw">:</span><span class="at"> best</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--pdf-engine xelatex</code></pre></td><td style="text-align: left;"><div id="cb186" class="sourceCode"><div class="sourceCode" id="cb70"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb70-1"><a href="#cb70-1" aria-hidden="true"></a><span class="fu">pdf-engine</span><span class="kw">:</span><span class="at"> xelatex</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--pdf-engine-opt=--shell-escape</code></pre></td><td style="text-align: left;"><div id="cb188" class="sourceCode"><div class="sourceCode" id="cb72"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb72-1"><a href="#cb72-1" aria-hidden="true"></a><span class="fu">pdf-engine-opts</span><span class="kw">:</span></span>
<span id="cb72-2"><a href="#cb72-2" aria-hidden="true"></a><span class="at">  </span><span class="kw">-</span><span class="at"> </span><span class="st">&#39;-shell-escape&#39;</span></span></code></pre></div></div><div id="cb189" class="sourceCode"><div class="sourceCode" id="cb73"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb73-1"><a href="#cb73-1" aria-hidden="true"></a><span class="fu">pdf-engine-opt</span><span class="kw">:</span><span class="at"> </span><span class="st">&#39;-shell-escape&#39;</span></span></code></pre></div></div></td></tr></tbody></table>

<a href="#citation-rendering-1" class="anchor"></a>Citation rendering <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------------------

<table style="width:98%;"><colgroup><col style="width: 48%" /><col style="width: 50%" /></colgroup><thead><tr class="header"><th style="text-align: left;">command line</th><th style="text-align: left;">defaults file</th></tr></thead><tbody><tr class="odd"><td style="text-align: left;"><pre><code>--citeproc</code></pre></td><td style="text-align: left;"><div id="cb191" class="sourceCode"><div class="sourceCode" id="cb2"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb2-1"><a href="#cb2-1" aria-hidden="true"></a><span class="fu">citeproc</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--bibliography logic.bib</code></pre></td><td style="text-align: left;"><div id="cb193" class="sourceCode"><div class="sourceCode" id="cb4"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb4-1"><a href="#cb4-1" aria-hidden="true"></a><span class="fu">bibliography</span><span class="kw">:</span><span class="at"> logic.bib</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--csl ieee.csl</code></pre></td><td style="text-align: left;"><div id="cb195" class="sourceCode"><div class="sourceCode" id="cb6"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb6-1"><a href="#cb6-1" aria-hidden="true"></a><span class="fu">csl</span><span class="kw">:</span><span class="at"> ieee.csl</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--citation-abbreviations ab.json</code></pre></td><td style="text-align: left;"><div id="cb197" class="sourceCode"><div class="sourceCode" id="cb8"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb8-1"><a href="#cb8-1" aria-hidden="true"></a><span class="fu">citation-abbreviations</span><span class="kw">:</span><span class="at"> ab.json</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--natbib</code></pre></td><td style="text-align: left;"><div id="cb199" class="sourceCode"><div class="sourceCode" id="cb10"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb10-1"><a href="#cb10-1" aria-hidden="true"></a><span class="fu">cite-method</span><span class="kw">:</span><span class="at"> natbib</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--biblatex</code></pre></td><td style="text-align: left;"><div id="cb201" class="sourceCode"><div class="sourceCode" id="cb12"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb12-1"><a href="#cb12-1" aria-hidden="true"></a><span class="fu">cite-method</span><span class="kw">:</span><span class="at"> biblatex</span></span></code></pre></div></div></td></tr></tbody></table>

<span class="nowrap">`cite-method`</span> can be <span class="nowrap">`citeproc`</span>, <span class="nowrap">`natbib`</span>, or <span class="nowrap">`biblatex`</span>. This only affects LaTeX output. If you want to use citeproc to format citations, you should also set ‘citeproc: true’.

If you need control over when the citeproc processing is done relative to other filters, you should instead use <span class="nowrap">`citeproc`</span> in the list of <span class="nowrap">`filters`</span> (see [Reader options](#reader-options-1)).

<a href="#math-rendering-in-html-1" class="anchor"></a>Math rendering in HTML <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------------------------

<table style="width:98%;"><colgroup><col style="width: 48%" /><col style="width: 50%" /></colgroup><thead><tr class="header"><th style="text-align: left;">command line</th><th style="text-align: left;">defaults file</th></tr></thead><tbody><tr class="odd"><td style="text-align: left;"><pre><code>--mathjax</code></pre></td><td style="text-align: left;"><div id="cb203" class="sourceCode"><div class="sourceCode" id="cb2"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb2-1"><a href="#cb2-1" aria-hidden="true"></a><span class="fu">html-math-method</span><span class="kw">:</span></span>
<span id="cb2-2"><a href="#cb2-2" aria-hidden="true"></a><span class="at">  </span><span class="fu">method</span><span class="kw">:</span><span class="at"> mathjax</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--mathml</code></pre></td><td style="text-align: left;"><div id="cb205" class="sourceCode"><div class="sourceCode" id="cb4"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb4-1"><a href="#cb4-1" aria-hidden="true"></a><span class="fu">html-math-method</span><span class="kw">:</span></span>
<span id="cb4-2"><a href="#cb4-2" aria-hidden="true"></a><span class="at">  </span><span class="fu">method</span><span class="kw">:</span><span class="at"> mathml</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--webtex</code></pre></td><td style="text-align: left;"><div id="cb207" class="sourceCode"><div class="sourceCode" id="cb6"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb6-1"><a href="#cb6-1" aria-hidden="true"></a><span class="fu">html-math-method</span><span class="kw">:</span></span>
<span id="cb6-2"><a href="#cb6-2" aria-hidden="true"></a><span class="at">  </span><span class="fu">method</span><span class="kw">:</span><span class="at"> webtex</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--katex</code></pre></td><td style="text-align: left;"><div id="cb209" class="sourceCode"><div class="sourceCode" id="cb8"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb8-1"><a href="#cb8-1" aria-hidden="true"></a><span class="fu">html-math-method</span><span class="kw">:</span></span>
<span id="cb8-2"><a href="#cb8-2" aria-hidden="true"></a><span class="at">  </span><span class="fu">method</span><span class="kw">:</span><span class="at"> katex</span></span></code></pre></div></div></td></tr><tr class="odd"><td style="text-align: left;"><pre><code>--gladtex</code></pre></td><td style="text-align: left;"><div id="cb211" class="sourceCode"><div class="sourceCode" id="cb10"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb10-1"><a href="#cb10-1" aria-hidden="true"></a><span class="fu">html-math-method</span><span class="kw">:</span></span>
<span id="cb10-2"><a href="#cb10-2" aria-hidden="true"></a><span class="at">  </span><span class="fu">method</span><span class="kw">:</span><span class="at"> gladtex</span></span></code></pre></div></div></td></tr></tbody></table>

In addition to the values listed above, <span class="nowrap">`method`</span> can have the value <span class="nowrap">`plain`</span>.

If the command line option accepts a URL argument, an <span class="nowrap">`url:`</span> field can be added to <span class="nowrap">`html-math-method:`</span>.

<a href="#options-for-wrapper-scripts-1" class="anchor"></a>Options for wrapper scripts <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------------------------------------

<table style="width:98%;"><colgroup><col style="width: 48%" /><col style="width: 50%" /></colgroup><thead><tr class="header"><th style="text-align: left;">command line</th><th style="text-align: left;">defaults file</th></tr></thead><tbody><tr class="odd"><td style="text-align: left;"><pre><code>--dump-args</code></pre></td><td style="text-align: left;"><div id="cb213" class="sourceCode"><div class="sourceCode" id="cb2"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb2-1"><a href="#cb2-1" aria-hidden="true"></a><span class="fu">dump-args</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr><tr class="even"><td style="text-align: left;"><pre><code>--ignore-args</code></pre></td><td style="text-align: left;"><div id="cb215" class="sourceCode"><div class="sourceCode" id="cb4"><pre class="sourceCode yaml"><code class="sourceCode yaml"><span id="cb4-1"><a href="#cb4-1" aria-hidden="true"></a><span class="fu">ignore-args</span><span class="kw">:</span><span class="at"> </span><span class="ch">true</span></span></code></pre></div></div></td></tr></tbody></table>

<a href="#templates" class="anchor"></a>Templates <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=================================================================================================================

When the <a href="#option--standalone" class="option"><span class="nowrap"><code>-s/--standalone</code></span></a> option is used, pandoc uses a template to add header and footer material that is needed for a self-standing document. To see the default template that is used, just type

    pandoc -D *FORMAT*

where *FORMAT* is the name of the output format. A custom template can be specified using the <a href="#option--template" class="option"><span class="nowrap"><code>--template</code></span></a> option. You can also override the system default templates for a given output format *FORMAT* by putting a file <span class="nowrap">`templates/default.*FORMAT*`</span> in the user data directory (see <a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>, above). *Exceptions:*

-   For <span class="nowrap">`odt`</span> output, customize the <span class="nowrap">`default.opendocument`</span> template.
-   For <span class="nowrap">`pdf`</span> output, customize the <span class="nowrap">`default.latex`</span> template (or the <span class="nowrap">`default.context`</span> template, if you use <a href="#option--to" class="option"><span class="nowrap"><code>-t context</code></span></a>, or the <span class="nowrap">`default.ms`</span> template, if you use <a href="#option--to" class="option"><span class="nowrap"><code>-t ms</code></span></a>, or the <span class="nowrap">`default.html`</span> template, if you use <a href="#option--to" class="option"><span class="nowrap"><code>-t html</code></span></a>).
-   <span class="nowrap">`docx`</span> and <span class="nowrap">`pptx`</span> have no template (however, you can use <span class="nowrap">`--reference-doc`</span> to customize the output).

Templates contain *variables*, which allow for the inclusion of arbitrary information at any point in the file. They may be set at the command line using the <a href="#option--variable" class="option"><span class="nowrap"><code>-V/--variable</code></span></a> option. If a variable is not set, pandoc will look for the key in the document’s metadata, which can be set using either [YAML metadata blocks](#extension-yaml_metadata_block) or with the <a href="#option--metadata" class="option"><span class="nowrap"><code>-M/--metadata</code></span></a> option. In addition, some variables are given default values by pandoc. See [Variables](#variables) below for a list of variables used in pandoc’s default templates.

If you use custom templates, you may need to revise them as pandoc changes. We recommend tracking the changes in the default templates, and modifying your custom templates accordingly. An easy way to do this is to fork the [pandoc-templates](https://github.com/jgm/pandoc-templates) repository and merge in changes after each pandoc release.

<a href="#template-syntax" class="anchor"></a>Template syntax <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------

### <a href="#comments" class="anchor"></a>Comments <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Anything between the sequence <span class="nowrap">`$--`</span> and the end of the line will be treated as a comment and omitted from the output.

### <a href="#delimiters" class="anchor"></a>Delimiters <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

To mark variables and control structures in the template, either <span class="nowrap">`$`</span>…<span class="nowrap">`$`</span> or <span class="nowrap">`${`</span>…<span class="nowrap">`}`</span> may be used as delimiters. The styles may also be mixed in the same template, but the opening and closing delimiter must match in each case. The opening delimiter may be followed by one or more spaces or tabs, which will be ignored. The closing delimiter may be preceded by one or more spaces or tabs, which will be ignored.

To include a literal <span class="nowrap">`$`</span> in the document, use <span class="nowrap">`$$`</span>.

### <a href="#interpolated-variables" class="anchor"></a>Interpolated variables <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A slot for an interpolated variable is a variable name surrounded by matched delimiters. Variable names must begin with a letter and can contain letters, numbers, <span class="nowrap">`_`</span>, <span class="nowrap">`-`</span>, and <span class="nowrap">`.`</span>. The keywords <span class="nowrap">`it`</span>, <span class="nowrap">`if`</span>, <span class="nowrap">`else`</span>, <span class="nowrap">`endif`</span>, <span class="nowrap">`for`</span>, <span class="nowrap">`sep`</span>, and <span class="nowrap">`endfor`</span> may not be used as variable names. Examples:

    $foo$
    $foo.bar.baz$
    $foo_bar.baz-bim$
    $ foo $
    ${foo}
    ${foo.bar.baz}
    ${foo_bar.baz-bim}
    ${ foo }

Variable names with periods are used to get at structured variable values. So, for example, <span class="nowrap">`employee.salary`</span> will return the value of the <span class="nowrap">`salary`</span> field of the object that is the value of the <span class="nowrap">`employee`</span> field.

-   If the value of the variable is a simple value, it will be rendered verbatim. (Note that no escaping is done; the assumption is that the calling program will escape the strings appropriately for the output format.)
-   If the value is a list, the values will be concatenated.
-   If the value is a map, the string <span class="nowrap">`true`</span> will be rendered.
-   Every other value will be rendered as the empty string.

### <a href="#conditionals" class="anchor"></a>Conditionals <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A conditional begins with <span class="nowrap">`if(variable)`</span> (enclosed in matched delimiters) and ends with <span class="nowrap">`endif`</span> (enclosed in matched delimiters). It may optionally contain an <span class="nowrap">`else`</span> (enclosed in matched delimiters). The <span class="nowrap">`if`</span> section is used if <span class="nowrap">`variable`</span> has a true value, otherwise the <span class="nowrap">`else`</span> section is used (if present). The following values count as true:

-   any map
-   any array containing at least one true value
-   any nonempty string
-   boolean True

Note that in YAML metadata (and metadata specified on the command line using <a href="#option--metadata" class="option"><span class="nowrap"><code>-M/--metadata</code></span></a>), unquoted <span class="nowrap">`true`</span> and <span class="nowrap">`false`</span> will be interpreted as Boolean values. But a variable specified on the command line using <a href="#option--variable" class="option"><span class="nowrap"><code>-V/--variable</code></span></a> will always be given a string value. Hence a conditional <span class="nowrap">`if(foo)`</span> will be triggered if you use <a href="#option--variable" class="option"><span class="nowrap"><code>-V foo=false</code></span></a>, but not if you use <a href="#option--metadata" class="option"><span class="nowrap"><code>-M foo=false</code></span></a>.

Examples:

    $if(foo)$bar$endif$

    $if(foo)$
      $foo$
    $endif$

    $if(foo)$
    part one
    $else$
    part two
    $endif$

    ${if(foo)}bar${endif}

    ${if(foo)}
      ${foo}
    ${endif}

    ${if(foo)}
    ${ foo.bar }
    ${else}
    no foo!
    ${endif}

The keyword <span class="nowrap">`elseif`</span> may be used to simplify complex nested conditionals:

    $if(foo)$
    XXX
    $elseif(bar)$
    YYY
    $else$
    ZZZ
    $endif$

### <a href="#for-loops" class="anchor"></a>For loops <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A for loop begins with <span class="nowrap">`for(variable)`</span> (enclosed in matched delimiters) and ends with <span class="nowrap">`endfor`</span> (enclosed in matched delimiters).

-   If <span class="nowrap">`variable`</span> is an array, the material inside the loop will be evaluated repeatedly, with <span class="nowrap">`variable`</span> being set to each value of the array in turn, and concatenated.
-   If <span class="nowrap">`variable`</span> is a map, the material inside will be set to the map.
-   If the value of the associated variable is not an array or a map, a single iteration will be performed on its value.

Examples:

    $for(foo)$$foo$$sep$, $endfor$

    $for(foo)$
      - $foo.last$, $foo.first$
    $endfor$

    ${ for(foo.bar) }
      - ${ foo.bar.last }, ${ foo.bar.first }
    ${ endfor }

    $for(mymap)$
    $it.name$: $it.office$
    $endfor$

You may optionally specify a separator between consecutive values using <span class="nowrap">`sep`</span> (enclosed in matched delimiters). The material between <span class="nowrap">`sep`</span> and the <span class="nowrap">`endfor`</span> is the separator.

    ${ for(foo) }${ foo }${ sep }, ${ endfor }

Instead of using <span class="nowrap">`variable`</span> inside the loop, the special anaphoric keyword <span class="nowrap">`it`</span> may be used.

    ${ for(foo.bar) }
      - ${ it.last }, ${ it.first }
    ${ endfor }

### <a href="#partials" class="anchor"></a>Partials <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Partials (subtemplates stored in different files) may be included by using the name of the partial, followed by <span class="nowrap">`()`</span>, for example:

    ${ styles() }

Partials will be sought in the directory containing the main template. The file name will be assumed to have the same extension as the main template if it lacks an extension. When calling the partial, the full name including file extension can also be used:

    ${ styles.html() }

(If a partial is not found in the directory of the template and the template path is given as a relative path, it will also be sought in the <span class="nowrap">`templates`</span> subdirectory of the user data directory.)

Partials may optionally be applied to variables using a colon:

    ${ date:fancy() }

    ${ articles:bibentry() }

If <span class="nowrap">`articles`</span> is an array, this will iterate over its values, applying the partial <span class="nowrap">`bibentry()`</span> to each one. So the second example above is equivalent to

    ${ for(articles) }
    ${ it:bibentry() }
    ${ endfor }

Note that the anaphoric keyword <span class="nowrap">`it`</span> must be used when iterating over partials. In the above examples, the <span class="nowrap">`bibentry`</span> partial should contain <span class="nowrap">`it.title`</span> (and so on) instead of <span class="nowrap">`articles.title`</span>.

Final newlines are omitted from included partials.

Partials may include other partials.

A separator between values of an array may be specified in square brackets, immediately after the variable name or partial:

    ${months[, ]}$

    ${articles:bibentry()[; ]$

The separator in this case is literal and (unlike with <span class="nowrap">`sep`</span> in an explicit <span class="nowrap">`for`</span> loop) cannot contain interpolated variables or other template directives.

### <a href="#nesting" class="anchor"></a>Nesting <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

To ensure that content is “nested,” that is, subsequent lines indented, use the <span class="nowrap">`^`</span> directive:

    $item.number$  $^$$item.description$ ($item.price$)

In this example, if <span class="nowrap">`item.description`</span> has multiple lines, they will all be indented to line up with the first line:

    00123  A fine bottle of 18-year old
           Oban whiskey. ($148)

To nest multiple lines to the same level, align them with the <span class="nowrap">`^`</span> directive in the template. For example:

    $item.number$  $^$$item.description$ ($item.price$)
                   (Available til $item.sellby$.)

will produce

    00123  A fine bottle of 18-year old
           Oban whiskey. ($148)
           (Available til March 30, 2020.)

If a variable occurs by itself on a line, preceded by whitespace and not followed by further text or directives on the same line, and the variable’s value contains multiple lines, it will be nested automatically.

### <a href="#breakable-spaces" class="anchor"></a>Breakable spaces <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Normally, spaces in the template itself (as opposed to values of the interpolated variables) are not breakable, but they can be made breakable in part of the template by using the <span class="nowrap">`~`</span> keyword (ended with another <span class="nowrap">`~`</span>).

    $~$This long line may break if the document is rendered
    with a short line length.$~$

### <a href="#pipes" class="anchor"></a>Pipes <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A pipe transforms the value of a variable or partial. Pipes are specified using a slash (<span class="nowrap">`/`</span>) between the variable name (or partial) and the pipe name. Example:

    $for(name)$
    $name/uppercase$
    $endfor$

    $for(metadata/pairs)$
    - $it.key$: $it.value$
    $endfor$

    $employee:name()/uppercase$

Pipes may be chained:

    $for(employees/pairs)$
    $it.key/alpha/uppercase$. $it.name$
    $endfor$

Some pipes take parameters:

    |----------------------|------------|
    $for(employee)$
    $it.name.first/uppercase/left 20 "| "$$it.name.salary/right 10 " | " " |"$
    $endfor$
    |----------------------|------------|

Currently the following pipes are predefined:

-   <span class="nowrap">`pairs`</span>: Converts a map or array to an array of maps, each with <span class="nowrap">`key`</span> and <span class="nowrap">`value`</span> fields. If the original value was an array, the <span class="nowrap">`key`</span> will be the array index, starting with 1.

-   <span class="nowrap">`uppercase`</span>: Converts text to uppercase.

-   <span class="nowrap">`lowercase`</span>: Converts text to lowercase.

-   <span class="nowrap">`length`</span>: Returns the length of the value: number of characters for a textual value, number of elements for a map or array.

-   <span class="nowrap">`reverse`</span>: Reverses a textual value or array, and has no effect on other values.

-   <span class="nowrap">`first`</span>: Returns the first value of an array, if applied to a non-empty array; otherwise returns the original value.

-   <span class="nowrap">`last`</span>: Returns the last value of an array, if applied to a non-empty array; otherwise returns the original value.

-   <span class="nowrap">`rest`</span>: Returns all but the first value of an array, if applied to a non-empty array; otherwise returns the original value.

-   <span class="nowrap">`allbutlast`</span>: Returns all but the last value of an array, if applied to a non-empty array; otherwise returns the original value.

-   <span class="nowrap">`chomp`</span>: Removes trailing newlines (and breakable space).

-   <span class="nowrap">`nowrap`</span>: Disables line wrapping on breakable spaces.

-   <span class="nowrap">`alpha`</span>: Converts textual values that can be read as an integer into lowercase alphabetic characters <span class="nowrap">`a..z`</span> (mod 26). This can be used to get lettered enumeration from array indices. To get uppercase letters, chain with <span class="nowrap">`uppercase`</span>.

-   <span class="nowrap">`roman`</span>: Converts textual values that can be read as an integer into lowercase roman numerals. This can be used to get lettered enumeration from array indices. To get uppercase roman, chain with <span class="nowrap">`uppercase`</span>.

-   <span class="nowrap">`left n "leftborder" "rightborder"`</span>: Renders a textual value in a block of width <span class="nowrap">`n`</span>, aligned to the left, with an optional left and right border. Has no effect on other values. This can be used to align material in tables. Widths are positive integers indicating the number of characters. Borders are strings inside double quotes; literal <span class="nowrap">`"`</span> and <span class="nowrap">`\`</span> characters must be backslash-escaped.

-   <span class="nowrap">`right n "leftborder" "rightborder"`</span>: Renders a textual value in a block of width <span class="nowrap">`n`</span>, aligned to the right, and has no effect on other values.

-   <span class="nowrap">`center n "leftborder" "rightborder"`</span>: Renders a textual value in a block of width <span class="nowrap">`n`</span>, aligned to the center, and has no effect on other values.

<a href="#variables" class="anchor"></a>Variables <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------

### <a href="#metadata-variables" class="anchor"></a>Metadata variables <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

<span class="nowrap">`title`</span>, <span class="nowrap">`author`</span>, <span class="nowrap">`date`</span>  
allow identification of basic aspects of the document. Included in PDF metadata through LaTeX and ConTeXt. These can be set through a [pandoc title block](#extension-pandoc_title_block), which allows for multiple authors, or through a [YAML metadata block](#extension-yaml_metadata_block):

    ---
    author:
    - Aristotle
    - Peter Abelard
    ...

Note that if you just want to set PDF or HTML metadata, without including a title block in the document itself, you can set the <span class="nowrap">`title-meta`</span>, <span class="nowrap">`author-meta`</span>, and <span class="nowrap">`date-meta`</span> variables. (By default these are set automatically, based on <span class="nowrap">`title`</span>, <span class="nowrap">`author`</span>, and <span class="nowrap">`date`</span>.) The page title in HTML is set by <span class="nowrap">`pagetitle`</span>, which is equal to <span class="nowrap">`title`</span> by default.

<span class="nowrap">`subtitle`</span>  
document subtitle, included in HTML, EPUB, LaTeX, ConTeXt, and docx documents

<span class="nowrap">`abstract`</span>  
document summary, included in HTML, LaTeX, ConTeXt, AsciiDoc, and docx documents

<span class="nowrap">`abstract-title`</span>  
title of abstract, currently used only in HTML, EPUB, and docx. This will be set automatically to a localized value, depending on <span class="nowrap">`lang`</span>, but can be manually overridden.

<span class="nowrap">`keywords`</span>  
list of keywords to be included in HTML, PDF, ODT, pptx, docx and AsciiDoc metadata; repeat as for <span class="nowrap">`author`</span>, above

<span class="nowrap">`subject`</span>  
document subject, included in ODT, PDF, docx, EPUB, and pptx metadata

<span class="nowrap">`description`</span>  
document description, included in ODT, docx and pptx metadata. Some applications show this as <span class="nowrap">`Comments`</span> metadata.

<span class="nowrap">`category`</span>  
document category, included in docx and pptx metadata

Additionally, any root-level string metadata, not included in ODT, docx or pptx metadata is added as a *custom property*. The following [YAML](https://yaml.org/spec/1.2/spec.html "YAML v1.2 Spec") metadata block for instance:

    ---
    title:  'This is the title'
    subtitle: "This is the subtitle"
    author:
    - Author One
    - Author Two
    description: |
        This is a long
        description.

        It consists of two paragraphs
    ...

will include <span class="nowrap">`title`</span>, <span class="nowrap">`author`</span> and <span class="nowrap">`description`</span> as standard document properties and <span class="nowrap">`subtitle`</span> as a custom property when converting to docx, ODT or pptx.

### <a href="#language-variables" class="anchor"></a>Language variables <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

<span class="nowrap">`lang`</span>  
identifies the main language of the document using IETF language tags (following the [BCP 47](https://tools.ietf.org/html/bcp47) standard), such as <span class="nowrap">`en`</span> or <span class="nowrap">`en-GB`</span>. The [Language subtag lookup](https://r12a.github.io/app-subtags/) tool can look up or verify these tags. This affects most formats, and controls hyphenation in PDF output when using LaTeX (through [<span class="nowrap">`babel`</span>](https://ctan.org/pkg/babel) and [<span class="nowrap">`polyglossia`</span>](https://ctan.org/pkg/polyglossia)) or ConTeXt.

Use native pandoc [Divs and Spans](#divs-and-spans) with the <span class="nowrap">`lang`</span> attribute to switch the language:

    ---
    lang: en-GB
    ...

    Text in the main document language (British English).

    ::: {lang=fr-CA}
    > Cette citation est écrite en français canadien.
    :::

    More text in English. ['Zitat auf Deutsch.']{lang=de}

<span class="nowrap">`dir`</span>  
the base script direction, either <span class="nowrap">`rtl`</span> (right-to-left) or <span class="nowrap">`ltr`</span> (left-to-right).

For bidirectional documents, native pandoc <span class="nowrap">`span`</span>s and <span class="nowrap">`div`</span>s with the <span class="nowrap">`dir`</span> attribute (value <span class="nowrap">`rtl`</span> or <span class="nowrap">`ltr`</span>) can be used to override the base direction in some output formats. This may not always be necessary if the final renderer (e.g. the browser, when generating HTML) supports the [Unicode Bidirectional Algorithm](https://www.w3.org/International/articles/inline-bidi-markup/uba-basics).

When using LaTeX for bidirectional documents, only the <span class="nowrap">`xelatex`</span> engine is fully supported (use <a href="#option--pdf-engine" class="option"><span class="nowrap"><code>--pdf-engine=xelatex</code></span></a>).

### <a href="#variables-for-html" class="anchor"></a>Variables for HTML <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

<span class="nowrap">`document-css`</span>  
Enables inclusion of most of the [CSS](https://developer.mozilla.org/en-US/docs/Learn/CSS) in the <span class="nowrap">`styles.html`</span> [partial](#partials) (have a look with <span class="nowrap">`pandoc --print-default-data-file=templates/styles.html`</span>). Unless you use <a href="#option--css" class="option"><span class="nowrap"><code>--css</code></span></a>, this variable is set to <span class="nowrap">`true`</span> by default. You can disable it with e.g. <span class="nowrap">`pandoc -M document-css=false`</span>.

<span class="nowrap">`mainfont`</span>  
sets the CSS <span class="nowrap">`font-family`</span> property on the <span class="nowrap">`html`</span> element.

<span class="nowrap">`fontsize`</span>  
sets the base CSS <span class="nowrap">`font-size`</span>, which you’d usually set to e.g. <span class="nowrap">`20px`</span>, but it also accepts <span class="nowrap">`pt`</span> (12pt = 16px in most browsers).

<span class="nowrap">`fontcolor`</span>  
sets the CSS <span class="nowrap">`color`</span> property on the <span class="nowrap">`html`</span> element.

<span class="nowrap">`linkcolor`</span>  
sets the CSS <span class="nowrap">`color`</span> property on all links.

<span class="nowrap">`monofont`</span>  
sets the CSS <span class="nowrap">`font-family`</span> property on <span class="nowrap">`code`</span> elements.

<span class="nowrap">`monobackgroundcolor`</span>  
sets the CSS <span class="nowrap">`background-color`</span> property on <span class="nowrap">`code`</span> elements and adds extra padding.

<span class="nowrap">`linestretch`</span>  
sets the CSS <span class="nowrap">`line-height`</span> property on the <span class="nowrap">`html`</span> element, which is preferred to be unitless.

<span class="nowrap">`maxwidth`</span>  
sets the CSS <span class="nowrap">`max-width`</span> property (default is 32em).

<span class="nowrap">`backgroundcolor`</span>  
sets the CSS <span class="nowrap">`background-color`</span> property on the <span class="nowrap">`html`</span> element.

<span class="nowrap">`margin-left`</span>, <span class="nowrap">`margin-right`</span>, <span class="nowrap">`margin-top`</span>, <span class="nowrap">`margin-bottom`</span>  
sets the corresponding CSS <span class="nowrap">`padding`</span> properties on the <span class="nowrap">`body`</span> element.

To override or extend some [CSS](https://developer.mozilla.org/en-US/docs/Learn/CSS) for just one document, include for example:

    ---
    header-includes: |
      <style>
      blockquote {
        font-style: italic;
      }
      tr.even {
        background-color: #f0f0f0;
      }
      td, th {
        padding: 0.5em 2em 0.5em 0.5em;
      }
      tbody {
        border-bottom: none;
      }
      </style>
    ---

### <a href="#variables-for-html-math" class="anchor"></a>Variables for HTML math <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

<span class="nowrap">`classoption`</span>  
when using <a href="#option--katex" class="option"><span class="nowrap"><code>--katex</code></span></a>, you can render display math equations flush left using [YAML metadata](#layout) or with <a href="#option--metadata" class="option"><span class="nowrap"><code>-M classoption=fleqn</code></span></a>.

### <a href="#variables-for-html-slides" class="anchor"></a>Variables for HTML slides <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

These affect HTML output when [producing slide shows with pandoc](#slide-shows).

<span class="nowrap">`institute`</span>  
author affiliations: can be a list when there are multiple authors

<span class="nowrap">`revealjs-url`</span>  
base URL for reveal.js documents (defaults to <span class="nowrap">`https://unpkg.com/reveal.js@^4/`</span>)

<span class="nowrap">`s5-url`</span>  
base URL for S5 documents (defaults to <span class="nowrap">`s5/default`</span>)

<span class="nowrap">`slidy-url`</span>  
base URL for Slidy documents (defaults to <span class="nowrap">`https://www.w3.org/Talks/Tools/Slidy2`</span>)

<span class="nowrap">`slideous-url`</span>  
base URL for Slideous documents (defaults to <span class="nowrap">`slideous`</span>)

<span class="nowrap">`title-slide-attributes`</span>  
additional attributes for the title slide of reveal.js slide shows. See [background in reveal.js, beamer, and pptx](#background-in-reveal.js-beamer-and-pptx) for an example.

All [reveal.js configuration options](https://revealjs.com/config/) are available as variables. To turn off boolean flags that default to true in reveal.js, use <span class="nowrap">`0`</span>.

### <a href="#variables-for-beamer-slides" class="anchor"></a>Variables for Beamer slides <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

These variables change the appearance of PDF slides using [<span class="nowrap">`beamer`</span>](https://ctan.org/pkg/beamer).

<span class="nowrap">`aspectratio`</span>  
slide aspect ratio (<span class="nowrap">`43`</span> for 4:3 \[default\], <span class="nowrap">`169`</span> for 16:9, <span class="nowrap">`1610`</span> for 16:10, <span class="nowrap">`149`</span> for 14:9, <span class="nowrap">`141`</span> for 1.41:1, <span class="nowrap">`54`</span> for 5:4, <span class="nowrap">`32`</span> for 3:2)

<span class="nowrap">`beameroption`</span>  
add extra beamer option with <span class="nowrap">`\setbeameroption{}`</span>

<span class="nowrap">`institute`</span>  
author affiliations: can be a list when there are multiple authors

<span class="nowrap">`logo`</span>  
logo image for slides

<span class="nowrap">`navigation`</span>  
controls navigation symbols (default is <span class="nowrap">`empty`</span> for no navigation symbols; other valid values are <span class="nowrap">`frame`</span>, <span class="nowrap">`vertical`</span>, and <span class="nowrap">`horizontal`</span>)

<span class="nowrap">`section-titles`</span>  
enables “title pages” for new sections (default is true)

<span class="nowrap">`theme`</span>, <span class="nowrap">`colortheme`</span>, <span class="nowrap">`fonttheme`</span>, <span class="nowrap">`innertheme`</span>, <span class="nowrap">`outertheme`</span>  
beamer themes

<span class="nowrap">`themeoptions`</span>, <span class="nowrap">`colorthemeoptions`</span>, <span class="nowrap">`fontthemeoptions`</span>, <span class="nowrap">`innerthemeoptions`</span>, <span class="nowrap">`outerthemeoptions`</span>  
options for LaTeX beamer themes (lists)

<span class="nowrap">`titlegraphic`</span>  
image for title slide: can be a list

<span class="nowrap">`titlegraphicoptions`</span>  
options for title slide image

<span class="nowrap">`shorttitle`</span>, <span class="nowrap">`shortsubtitle`</span>, <span class="nowrap">`shortauthor`</span>, <span class="nowrap">`shortinstitute`</span>, <span class="nowrap">`shortdate`</span>  
some beamer themes use short versions of the title, subtitle, author, institute, date

### <a href="#variables-for-powerpoint" class="anchor"></a>Variables for PowerPoint <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

These variables control the visual aspects of a slide show that are not easily controlled via templates.

<span class="nowrap">`monofont`</span>  
font to use for code.

### <a href="#variables-for-latex" class="anchor"></a>Variables for LaTeX <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Pandoc uses these variables when [creating a PDF](#creating-a-pdf) with a LaTeX engine.

#### <a href="#layout" class="anchor"></a>Layout <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

<span class="nowrap">`block-headings`</span>  
make <span class="nowrap">`\paragraph`</span> and <span class="nowrap">`\subparagraph`</span> (fourth- and fifth-level headings, or fifth- and sixth-level with book classes) free-standing rather than run-in; requires further formatting to distinguish from <span class="nowrap">`\subsubsection`</span> (third- or fourth-level headings). Instead of using this option, [KOMA-Script](https://ctan.org/pkg/koma-script) can adjust headings more extensively:

    ---
    documentclass: scrartcl
    header-includes: |
      \RedeclareSectionCommand[
        beforeskip=-10pt plus -2pt minus -1pt,
        afterskip=1sp plus -1sp minus 1sp,
        font=\normalfont\itshape]{paragraph}
      \RedeclareSectionCommand[
        beforeskip=-10pt plus -2pt minus -1pt,
        afterskip=1sp plus -1sp minus 1sp,
        font=\normalfont\scshape,
        indent=0pt]{subparagraph}
    ...

<span class="nowrap">`classoption`</span>  
option for document class, e.g. <span class="nowrap">`oneside`</span>; repeat for multiple options:

    ---
    classoption:
    - twocolumn
    - landscape
    ...

<span class="nowrap">`documentclass`</span>  
document class: usually one of the standard classes, [<span class="nowrap">`article`</span>](https://ctan.org/pkg/article), [<span class="nowrap">`book`</span>](https://ctan.org/pkg/book), and [<span class="nowrap">`report`</span>](https://ctan.org/pkg/report); the [KOMA-Script](https://ctan.org/pkg/koma-script) equivalents, <span class="nowrap">`scrartcl`</span>, <span class="nowrap">`scrbook`</span>, and <span class="nowrap">`scrreprt`</span>, which default to smaller margins; or [<span class="nowrap">`memoir`</span>](https://ctan.org/pkg/memoir)

<span class="nowrap">`geometry`</span>  
option for [<span class="nowrap">`geometry`</span>](https://ctan.org/pkg/geometry) package, e.g. <span class="nowrap">`margin=1in`</span>; repeat for multiple options:

    ---
    geometry:
    - top=30mm
    - left=20mm
    - heightrounded
    ...

<span class="nowrap">`hyperrefoptions`</span>  
option for [<span class="nowrap">`hyperref`</span>](https://ctan.org/pkg/hyperref) package, e.g. <span class="nowrap">`linktoc=all`</span>; repeat for multiple options:

    ---
    hyperrefoptions:
    - linktoc=all
    - pdfwindowui
    - pdfpagemode=FullScreen
    ...

<span class="nowrap">`indent`</span>  
if true, pandoc will use document class settings for indentation (the default LaTeX template otherwise removes indentation and adds space between paragraphs)

<span class="nowrap">`linestretch`</span>  
adjusts line spacing using the [<span class="nowrap">`setspace`</span>](https://ctan.org/pkg/setspace) package, e.g. <span class="nowrap">`1.25`</span>, <span class="nowrap">`1.5`</span>

<span class="nowrap">`margin-left`</span>, <span class="nowrap">`margin-right`</span>, <span class="nowrap">`margin-top`</span>, <span class="nowrap">`margin-bottom`</span>  
sets margins if <span class="nowrap">`geometry`</span> is not used (otherwise <span class="nowrap">`geometry`</span> overrides these)

<span class="nowrap">`pagestyle`</span>  
control <span class="nowrap">`\pagestyle{}`</span>: the default article class supports <span class="nowrap">`plain`</span> (default), <span class="nowrap">`empty`</span> (no running heads or page numbers), and <span class="nowrap">`headings`</span> (section titles in running heads)

<span class="nowrap">`papersize`</span>  
paper size, e.g. <span class="nowrap">`letter`</span>, <span class="nowrap">`a4`</span>

<span class="nowrap">`secnumdepth`</span>  
numbering depth for sections (with <a href="#option--number-sections" class="option"><span class="nowrap"><code>--number-sections</code></span></a> option or <span class="nowrap">`numbersections`</span> variable)

<span class="nowrap">`beamerarticle`</span>  
produce an article from Beamer slides. Note: if you set this variable, you must specify the beamer writer but use the default *LaTeX* template: for example, <span class="nowrap">`pandoc -Vbeamerarticle -t beamer --template default.latex`</span>.

<span class="nowrap">`handout`</span>  
produce a handout version of Beamer slides (with overlays condensed into single slides)

#### <a href="#fonts" class="anchor"></a>Fonts <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

<span class="nowrap">`fontenc`</span>  
allows font encoding to be specified through <span class="nowrap">`fontenc`</span> package (with <span class="nowrap">`pdflatex`</span>); default is <span class="nowrap">`T1`</span> (see [LaTeX font encodings guide](https://ctan.org/pkg/encguide))

<span class="nowrap">`fontfamily`</span>  
font package for use with <span class="nowrap">`pdflatex`</span>: [TeX Live](https://www.tug.org/texlive/) includes many options, documented in the [LaTeX Font Catalogue](https://tug.org/FontCatalogue/). The default is [Latin Modern](https://ctan.org/pkg/lm).

<span class="nowrap">`fontfamilyoptions`</span>  
options for package used as <span class="nowrap">`fontfamily`</span>; repeat for multiple options. For example, to use the Libertine font with proportional lowercase (old-style) figures through the [<span class="nowrap">`libertinus`</span>](https://ctan.org/pkg/libertinus) package:

    ---
    fontfamily: libertinus
    fontfamilyoptions:
    - osf
    - p
    ...

<span class="nowrap">`fontsize`</span>  
font size for body text. The standard classes allow 10pt, 11pt, and 12pt. To use another size, set <span class="nowrap">`documentclass`</span> to one of the [KOMA-Script](https://ctan.org/pkg/koma-script) classes, such as <span class="nowrap">`scrartcl`</span> or <span class="nowrap">`scrbook`</span>.

<span class="nowrap">`mainfont`</span>, <span class="nowrap">`sansfont`</span>, <span class="nowrap">`monofont`</span>, <span class="nowrap">`mathfont`</span>, <span class="nowrap">`CJKmainfont`</span>, <span class="nowrap">`CJKsansfont`</span>, <span class="nowrap">`CJKmonofont`</span>  
font families for use with <span class="nowrap">`xelatex`</span> or <span class="nowrap">`lualatex`</span>: take the name of any system font, using the [<span class="nowrap">`fontspec`</span>](https://ctan.org/pkg/fontspec) package. <span class="nowrap">`CJKmainfont`</span> uses the [<span class="nowrap">`xecjk`</span>](https://ctan.org/pkg/xecjk) package if <span class="nowrap">`xelatex`</span> is used, or the [<span class="nowrap">`luatexja`</span>](https://ctan.org/pkg/luatexja) package if <span class="nowrap">`lualatex`</span> is used.

<span class="nowrap">`mainfontoptions`</span>, <span class="nowrap">`sansfontoptions`</span>, <span class="nowrap">`monofontoptions`</span>, <span class="nowrap">`mathfontoptions`</span>, <span class="nowrap">`CJKoptions`</span>, <span class="nowrap">`luatexjapresetoptions`</span>  
options to use with <span class="nowrap">`mainfont`</span>, <span class="nowrap">`sansfont`</span>, <span class="nowrap">`monofont`</span>, <span class="nowrap">`mathfont`</span>, <span class="nowrap">`CJKmainfont`</span> in <span class="nowrap">`xelatex`</span> and <span class="nowrap">`lualatex`</span>. Allow for any choices available through [<span class="nowrap">`fontspec`</span>](https://ctan.org/pkg/fontspec); repeat for multiple options. For example, to use the [TeX Gyre](http://www.gust.org.pl/projects/e-foundry/tex-gyre) version of Palatino with lowercase figures:

    ---
    mainfont: TeX Gyre Pagella
    mainfontoptions:
    - Numbers=Lowercase
    - Numbers=Proportional
    ...

<span class="nowrap">`mainfontfallback`</span>, <span class="nowrap">`sansfontfallback`</span>, <span class="nowrap">`monofontfallback`</span>  
fonts to try if a glyph isn’t found in <span class="nowrap">`mainfont`</span>, <span class="nowrap">`sansfont`</span>, or <span class="nowrap">`monofont`</span> respectively. These are lists. The font name must be followed by a colon and optionally a set of options, for example:

    ---
    mainfontfallback:
      - "FreeSans:"
      - "NotoColorEmoji:mode=harf"
    ...

Font fallbacks currently only work with <span class="nowrap">`lualatex`</span>.

<span class="nowrap">`babelfonts`</span>  
a map of Babel language names (e.g. <span class="nowrap">`chinese`</span>) to the font to be used with the language:

    ---
    babelfonts:
      chinese-hant: "Noto Serif CJK TC"
      russian: "Noto Serif"
    ...

<span class="nowrap">`microtypeoptions`</span>  
options to pass to the microtype package

#### <a href="#links" class="anchor"></a>Links <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

<span class="nowrap">`colorlinks`</span>  
add color to link text; automatically enabled if any of <span class="nowrap">`linkcolor`</span>, <span class="nowrap">`filecolor`</span>, <span class="nowrap">`citecolor`</span>, <span class="nowrap">`urlcolor`</span>, or <span class="nowrap">`toccolor`</span> are set

<span class="nowrap">`boxlinks`</span>  
add visible box around links (has no effect if <span class="nowrap">`colorlinks`</span> is set)

<span class="nowrap">`linkcolor`</span>, <span class="nowrap">`filecolor`</span>, <span class="nowrap">`citecolor`</span>, <span class="nowrap">`urlcolor`</span>, <span class="nowrap">`toccolor`</span>  
color for internal links, external links, citation links, linked URLs, and links in table of contents, respectively: uses options allowed by [<span class="nowrap">`xcolor`</span>](https://ctan.org/pkg/xcolor), including the <span class="nowrap">`dvipsnames`</span>, <span class="nowrap">`svgnames`</span>, and <span class="nowrap">`x11names`</span> lists

<span class="nowrap">`links-as-notes`</span>  
causes links to be printed as footnotes

<span class="nowrap">`urlstyle`</span>  
style for URLs (e.g., <span class="nowrap">`tt`</span>, <span class="nowrap">`rm`</span>, <span class="nowrap">`sf`</span>, and, the default, <span class="nowrap">`same`</span>)

#### <a href="#front-matter" class="anchor"></a>Front matter <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

<span class="nowrap">`lof`</span>, <span class="nowrap">`lot`</span>  
include list of figures, list of tables (can also be set using <a href="#option--lof%5B" class="option"><span class="nowrap"><code>--lof/--list-of-figures</code></span></a>, <a href="#option--lot%5B" class="option"><span class="nowrap"><code>--lot/--list-of-tables</code></span></a>)

<span class="nowrap">`thanks`</span>  
contents of acknowledgments footnote after document title

<span class="nowrap">`toc`</span>  
include table of contents (can also be set using <a href="#option--toc%5B" class="option"><span class="nowrap"><code>--toc/--table-of-contents</code></span></a>)

<span class="nowrap">`toc-depth`</span>  
level of section to include in table of contents

#### <a href="#biblatex-bibliographies" class="anchor"></a>BibLaTeX Bibliographies <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

These variables function when using BibLaTeX for [citation rendering](#citation-rendering).

<span class="nowrap">`biblatexoptions`</span>  
list of options for biblatex

<span class="nowrap">`biblio-style`</span>  
bibliography style, when used with <a href="#option--natbib" class="option"><span class="nowrap"><code>--natbib</code></span></a> and <a href="#option--biblatex" class="option"><span class="nowrap"><code>--biblatex</code></span></a>

<span class="nowrap">`biblio-title`</span>  
bibliography title, when used with <a href="#option--natbib" class="option"><span class="nowrap"><code>--natbib</code></span></a> and <a href="#option--biblatex" class="option"><span class="nowrap"><code>--biblatex</code></span></a>

<span class="nowrap">`bibliography`</span>  
bibliography to use for resolving references

<span class="nowrap">`natbiboptions`</span>  
list of options for natbib

### <a href="#variables-for-context" class="anchor"></a>Variables for ConTeXt <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Pandoc uses these variables when [creating a PDF](#creating-a-pdf) with ConTeXt.

<span class="nowrap">`fontsize`</span>  
font size for body text (e.g. <span class="nowrap">`10pt`</span>, <span class="nowrap">`12pt`</span>)

<span class="nowrap">`headertext`</span>, <span class="nowrap">`footertext`</span>  
text to be placed in running header or footer (see [ConTeXt Headers and Footers](https://wiki.contextgarden.net/Headers_and_Footers)); repeat up to four times for different placement

<span class="nowrap">`indenting`</span>  
controls indentation of paragraphs, e.g. <span class="nowrap">`yes,small,next`</span> (see [ConTeXt Indentation](https://wiki.contextgarden.net/Indentation)); repeat for multiple options

<span class="nowrap">`interlinespace`</span>  
adjusts line spacing, e.g. <span class="nowrap">`4ex`</span> (using [<span class="nowrap">`setupinterlinespace`</span>](https://wiki.contextgarden.net/Command/setupinterlinespace)); repeat for multiple options

<span class="nowrap">`layout`</span>  
options for page margins and text arrangement (see [ConTeXt Layout](https://wiki.contextgarden.net/Layout)); repeat for multiple options

<span class="nowrap">`linkcolor`</span>, <span class="nowrap">`contrastcolor`</span>  
color for links outside and inside a page, e.g. <span class="nowrap">`red`</span>, <span class="nowrap">`blue`</span> (see [ConTeXt Color](https://wiki.contextgarden.net/Color))

<span class="nowrap">`linkstyle`</span>  
typeface style for links, e.g. <span class="nowrap">`normal`</span>, <span class="nowrap">`bold`</span>, <span class="nowrap">`slanted`</span>, <span class="nowrap">`boldslanted`</span>, <span class="nowrap">`type`</span>, <span class="nowrap">`cap`</span>, <span class="nowrap">`small`</span>

<span class="nowrap">`lof`</span>, <span class="nowrap">`lot`</span>  
include list of figures, list of tables

<span class="nowrap">`mainfont`</span>, <span class="nowrap">`sansfont`</span>, <span class="nowrap">`monofont`</span>, <span class="nowrap">`mathfont`</span>  
font families: take the name of any system font (see [ConTeXt Font Switching](https://wiki.contextgarden.net/Font_Switching))

<span class="nowrap">`mainfontfallback`</span>, <span class="nowrap">`sansfontfallback`</span>, <span class="nowrap">`monofontfallback`</span>  
list of fonts to try, in order, if a glyph is not found in the main font. Use <span class="nowrap">`\definefallbackfamily`</span>-compatible font name syntax. Emoji fonts are unsupported.

<span class="nowrap">`margin-left`</span>, <span class="nowrap">`margin-right`</span>, <span class="nowrap">`margin-top`</span>, <span class="nowrap">`margin-bottom`</span>  
sets margins, if <span class="nowrap">`layout`</span> is not used (otherwise <span class="nowrap">`layout`</span> overrides these)

<span class="nowrap">`pagenumbering`</span>  
page number style and location (using [<span class="nowrap">`setuppagenumbering`</span>](https://wiki.contextgarden.net/Command/setuppagenumbering)); repeat for multiple options

<span class="nowrap">`papersize`</span>  
paper size, e.g. <span class="nowrap">`letter`</span>, <span class="nowrap">`A4`</span>, <span class="nowrap">`landscape`</span> (see [ConTeXt Paper Setup](https://wiki.contextgarden.net/PaperSetup)); repeat for multiple options

<span class="nowrap">`pdfa`</span>  
adds to the preamble the setup necessary to generate PDF/A of the type specified, e.g. <span class="nowrap">`1a:2005`</span>, <span class="nowrap">`2a`</span>. If no type is specified (i.e. the value is set to True, by e.g. <a href="#option--metadata" class="option"><span class="nowrap"><code>--metadata=pdfa</code></span></a> or <span class="nowrap">`pdfa: true`</span> in a YAML metadata block), <span class="nowrap">`1b:2005`</span> will be used as default, for reasons of backwards compatibility. Using <a href="#option--variable" class="option"><span class="nowrap"><code>--variable=pdfa</code></span></a> without specified value is not supported. To successfully generate PDF/A the required ICC color profiles have to be available and the content and all included files (such as images) have to be standard-conforming. The ICC profiles and output intent may be specified using the variables <span class="nowrap">`pdfaiccprofile`</span> and <span class="nowrap">`pdfaintent`</span>. See also [ConTeXt PDFA](https://wiki.contextgarden.net/PDF/A) for more details.

<span class="nowrap">`pdfaiccprofile`</span>  
when used in conjunction with <span class="nowrap">`pdfa`</span>, specifies the ICC profile to use in the PDF, e.g. <span class="nowrap">`default.cmyk`</span>. If left unspecified, <span class="nowrap">`sRGB.icc`</span> is used as default. May be repeated to include multiple profiles. Note that the profiles have to be available on the system. They can be obtained from [ConTeXt ICC Profiles](https://wiki.contextgarden.net/PDFX#ICC_profiles).

<span class="nowrap">`pdfaintent`</span>  
when used in conjunction with <span class="nowrap">`pdfa`</span>, specifies the output intent for the colors, e.g. <span class="nowrap">`ISO coated v2 300\letterpercent\space (ECI)`</span> If left unspecified, <span class="nowrap">`sRGB IEC61966-2.1`</span> is used as default.

<span class="nowrap">`toc`</span>  
include table of contents (can also be set using <a href="#option--toc%5B" class="option"><span class="nowrap"><code>--toc/--table-of-contents</code></span></a>)

<span class="nowrap">`urlstyle`</span>  
typeface style for links without link text, e.g. <span class="nowrap">`normal`</span>, <span class="nowrap">`bold`</span>, <span class="nowrap">`slanted`</span>, <span class="nowrap">`boldslanted`</span>, <span class="nowrap">`type`</span>, <span class="nowrap">`cap`</span>, <span class="nowrap">`small`</span>

<span class="nowrap">`whitespace`</span>  
spacing between paragraphs, e.g. <span class="nowrap">`none`</span>, <span class="nowrap">`small`</span> (using [<span class="nowrap">`setupwhitespace`</span>](https://wiki.contextgarden.net/Command/setupwhitespace))

<span class="nowrap">`includesource`</span>  
include all source documents as file attachments in the PDF file

### <a href="#variables-for-wkhtmltopdf" class="anchor"></a>Variables for <span class="nowrap">`wkhtmltopdf`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Pandoc uses these variables when [creating a PDF](#creating-a-pdf) with [<span class="nowrap">`wkhtmltopdf`</span>](https://wkhtmltopdf.org). The <a href="#option--css" class="option"><span class="nowrap"><code>--css</code></span></a> option also affects the output.

<span class="nowrap">`footer-html`</span>, <span class="nowrap">`header-html`</span>  
add information to the header and footer

<span class="nowrap">`margin-left`</span>, <span class="nowrap">`margin-right`</span>, <span class="nowrap">`margin-top`</span>, <span class="nowrap">`margin-bottom`</span>  
set the page margins

<span class="nowrap">`papersize`</span>  
sets the PDF paper size

### <a href="#variables-for-man-pages" class="anchor"></a>Variables for man pages <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

<span class="nowrap">`adjusting`</span>  
adjusts text to left (<span class="nowrap">`l`</span>), right (<span class="nowrap">`r`</span>), center (<span class="nowrap">`c`</span>), or both (<span class="nowrap">`b`</span>) margins

<span class="nowrap">`footer`</span>  
footer in man pages

<span class="nowrap">`header`</span>  
header in man pages

<span class="nowrap">`section`</span>  
section number in man pages

### <a href="#variables-for-texinfo" class="anchor"></a>Variables for Texinfo <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

<span class="nowrap">`version`</span>  
version of software (used in title and title page)

<span class="nowrap">`filename`</span>  
name of info file to be generated (defaults to a name based on the texi filename)

### <a href="#variables-for-typst" class="anchor"></a>Variables for Typst <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

<span class="nowrap">`margin`</span>  
A dictionary with the fields defined in the Typst documentation: <span class="nowrap">`x`</span>, <span class="nowrap">`y`</span>, <span class="nowrap">`top`</span>, <span class="nowrap">`bottom`</span>, <span class="nowrap">`left`</span>, <span class="nowrap">`right`</span>.

<span class="nowrap">`papersize`</span>  
Paper size: <span class="nowrap">`a4`</span>, <span class="nowrap">`us-letter`</span>, etc.

<span class="nowrap">`mainfont`</span>  
Name of system font to use for the main font.

<span class="nowrap">`fontsize`</span>  
Font size (e.g., <span class="nowrap">`12pt`</span>).

<span class="nowrap">`section-numbering`</span>  
Schema to use for numbering sections, e.g. <span class="nowrap">`1.A.1`</span>.

<span class="nowrap">`columns`</span>  
Number of columns for body text.

### <a href="#variables-for-ms" class="anchor"></a>Variables for ms <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

<span class="nowrap">`fontfamily`</span>  
<span class="nowrap">`A`</span> (Avant Garde), <span class="nowrap">`B`</span> (Bookman), <span class="nowrap">`C`</span> (Helvetica), <span class="nowrap">`HN`</span> (Helvetica Narrow), <span class="nowrap">`P`</span> (Palatino), or <span class="nowrap">`T`</span> (Times New Roman). This setting does not affect source code, which is always displayed using monospace Courier. These built-in fonts are limited in their coverage of characters. Additional fonts may be installed using the script [<span class="nowrap">`install-font.sh`</span>](https://www.schaffter.ca/mom/bin/install-font.sh) provided by Peter Schaffter and documented in detail on [his web site](https://www.schaffter.ca/mom/momdoc/appendices.html#steps).

<span class="nowrap">`indent`</span>  
paragraph indent (e.g. <span class="nowrap">`2m`</span>)

<span class="nowrap">`lineheight`</span>  
line height (e.g. <span class="nowrap">`12p`</span>)

<span class="nowrap">`pointsize`</span>  
point size (e.g. <span class="nowrap">`10p`</span>)

### <a href="#variables-set-automatically" class="anchor"></a>Variables set automatically <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Pandoc sets these variables automatically in response to [options](#options) or document contents; users can also modify them. These vary depending on the output format, and include the following:

<span class="nowrap">`body`</span>  
body of document

<span class="nowrap">`date-meta`</span>  
the <span class="nowrap">`date`</span> variable converted to ISO 8601 YYYY-MM-DD, included in all HTML based formats (dzslides, epub, html, html4, html5, revealjs, s5, slideous, slidy). The recognized formats for <span class="nowrap">`date`</span> are: <span class="nowrap">`mm/dd/yyyy`</span>, <span class="nowrap">`mm/dd/yy`</span>, <span class="nowrap">`yyyy-mm-dd`</span> (ISO 8601), <span class="nowrap">`dd MM yyyy`</span> (e.g. either <span class="nowrap">`02 Apr 2018`</span> or <span class="nowrap">`02 April 2018`</span>), <span class="nowrap">`MM dd, yyyy`</span> (e.g. <span class="nowrap">`Apr. 02, 2018`</span> or <span class="nowrap">`April 02, 2018),`</span>yyyy\[mm\[dd\]\]<span class="nowrap">`(e.g.`</span>20180402, <span class="nowrap">`201804`</span> or <span class="nowrap">`2018`</span>).

<span class="nowrap">`header-includes`</span>  
contents specified by <a href="#option--include-in-header" class="option"><span class="nowrap"><code>-H/--include-in-header</code></span></a> (may have multiple values)

<span class="nowrap">`include-before`</span>  
contents specified by <a href="#option--include-before-body" class="option"><span class="nowrap"><code>-B/--include-before-body</code></span></a> (may have multiple values)

<span class="nowrap">`include-after`</span>  
contents specified by <a href="#option--include-after-body" class="option"><span class="nowrap"><code>-A/--include-after-body</code></span></a> (may have multiple values)

<span class="nowrap">`meta-json`</span>  
JSON representation of all of the document’s metadata. Field values are transformed to the selected output format.

<span class="nowrap">`numbersections`</span>  
non-null value if <a href="#option--number-sections" class="option"><span class="nowrap"><code>-N/--number-sections</code></span></a> was specified

<span class="nowrap">`sourcefile`</span>, <span class="nowrap">`outputfile`</span>  
source and destination filenames, as given on the command line. <span class="nowrap">`sourcefile`</span> can also be a list if input comes from multiple files, or empty if input is from stdin. You can use the following snippet in your template to distinguish them:

    $if(sourcefile)$
    $for(sourcefile)$
    $sourcefile$
    $endfor$
    $else$
    (stdin)
    $endif$

Similarly, <span class="nowrap">`outputfile`</span> can be <span class="nowrap">`-`</span> if output goes to the terminal.

If you need absolute paths, use e.g. <span class="nowrap">`$curdir$/$sourcefile$`</span>.

<span class="nowrap">`curdir`</span>  
working directory from which pandoc is run.

<span class="nowrap">`pandoc-version`</span>  
pandoc version.

<span class="nowrap">`toc`</span>  
non-null value if <a href="#option--toc%5B" class="option"><span class="nowrap"><code>--toc/--table-of-contents</code></span></a> was specified

<span class="nowrap">`toc-title`</span>  
title of table of contents (works only with EPUB, HTML, revealjs, opendocument, odt, docx, pptx, beamer, LaTeX). Note that in docx and pptx a custom <span class="nowrap">`toc-title`</span> will be picked up from metadata, but cannot be set as a variable.

<a href="#extensions" class="anchor"></a>Extensions <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
===================================================================================================================

The behavior of some of the readers and writers can be adjusted by enabling or disabling various extensions.

An extension can be enabled by adding <span class="nowrap">`+EXTENSION`</span> to the format name and disabled by adding <span class="nowrap">`-EXTENSION`</span>. For example, <a href="#option--from" class="option"><span class="nowrap"><code>--from markdown_strict+footnotes</code></span></a> is strict Markdown with footnotes enabled, while <a href="#option--from" class="option"><span class="nowrap"><code>--from markdown-footnotes-pipe_tables</code></span></a> is pandoc’s Markdown without footnotes or pipe tables.

The Markdown reader and writer make by far the most use of extensions. Extensions only used by them are therefore covered in the section [Pandoc’s Markdown](#pandocs-markdown) below (see [Markdown variants](#markdown-variants) for <span class="nowrap">`commonmark`</span> and <span class="nowrap">`gfm`</span>). In the following, extensions that also work for other formats are covered.

Note that Markdown extensions added to the <span class="nowrap">`ipynb`</span> format affect Markdown cells in Jupyter notebooks (as do command-line options like <a href="#option--markdown-headings" class="option"><span class="nowrap"><code>--markdown-headings</code></span></a>).

<a href="#typography" class="anchor"></a>Typography <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------

### <a href="#extension-smart" class="anchor"></a>Extension: <span class="nowrap">`smart`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Interpret straight quotes as curly quotes, <span class="nowrap">`---`</span> as em-dashes, <span class="nowrap">`--`</span> as en-dashes, and <span class="nowrap">`...`</span> as ellipses. Nonbreaking spaces are inserted after certain abbreviations, such as “Mr.”

This extension can be enabled/disabled for the following formats:

input formats  
<span class="nowrap">`markdown`</span>, <span class="nowrap">`commonmark`</span>, <span class="nowrap">`latex`</span>, <span class="nowrap">`mediawiki`</span>, <span class="nowrap">`org`</span>, <span class="nowrap">`rst`</span>, <span class="nowrap">`twiki`</span>, <span class="nowrap">`html`</span>

output formats  
<span class="nowrap">`markdown`</span>, <span class="nowrap">`latex`</span>, <span class="nowrap">`context`</span>, <span class="nowrap">`rst`</span>

enabled by default in  
<span class="nowrap">`markdown`</span>, <span class="nowrap">`latex`</span>, <span class="nowrap">`context`</span> (both input and output)

Note: If you are *writing* Markdown, then the <span class="nowrap">`smart`</span> extension has the reverse effect: what would have been curly quotes comes out straight.

In LaTeX, <span class="nowrap">`smart`</span> means to use the standard TeX ligatures for quotation marks (<span class="nowrap">``` `` ```</span> and <span class="nowrap">`''`</span> for double quotes, <span class="nowrap">`` ` ``</span> and <span class="nowrap">`'`</span> for single quotes) and dashes (<span class="nowrap">`--`</span> for en-dash and <span class="nowrap">`---`</span> for em-dash). If <span class="nowrap">`smart`</span> is disabled, then in reading LaTeX pandoc will parse these characters literally. In writing LaTeX, enabling <span class="nowrap">`smart`</span> tells pandoc to use the ligatures when possible; if <span class="nowrap">`smart`</span> is disabled pandoc will use unicode quotation mark and dash characters.

<a href="#headings-and-sections" class="anchor"></a>Headings and sections <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------------------

### <a href="#extension-auto_identifiers" class="anchor"></a>Extension: <span class="nowrap">`auto_identifiers`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A heading without an explicitly specified identifier will be automatically assigned a unique identifier based on the heading text.

This extension can be enabled/disabled for the following formats:

input formats  
<span class="nowrap">`markdown`</span>, <span class="nowrap">`latex`</span>, <span class="nowrap">`rst`</span>, <span class="nowrap">`mediawiki`</span>, <span class="nowrap">`textile`</span>

output formats  
<span class="nowrap">`markdown`</span>, <span class="nowrap">`muse`</span>

enabled by default in  
<span class="nowrap">`markdown`</span>, <span class="nowrap">`muse`</span>

The default algorithm used to derive the identifier from the heading text is:

-   Remove all formatting, links, etc.
-   Remove all footnotes.
-   Remove all non-alphanumeric characters, except underscores, hyphens, and periods.
-   Replace all spaces and newlines with hyphens.
-   Convert all alphabetic characters to lowercase.
-   Remove everything up to the first letter (identifiers may not begin with a number or punctuation mark).
-   If nothing is left after this, use the identifier <span class="nowrap">`section`</span>.

Thus, for example,

<table><thead><tr class="header"><th style="text-align: left;">Heading</th><th style="text-align: left;">Identifier</th></tr></thead><tbody><tr class="odd"><td style="text-align: left;"><span class="nowrap"><code>Heading identifiers in HTML</code></span></td><td style="text-align: left;"><span class="nowrap"><code>heading-identifiers-in-html</code></span></td></tr><tr class="even"><td style="text-align: left;"><span class="nowrap"><code>Maître d'hôtel</code></span></td><td style="text-align: left;"><span class="nowrap"><code>maître-dhôtel</code></span></td></tr><tr class="odd"><td style="text-align: left;"><span class="nowrap"><code>*Dogs*?--in *my* house?</code></span></td><td style="text-align: left;"><span class="nowrap"><code>dogs--in-my-house</code></span></td></tr><tr class="even"><td style="text-align: left;"><span class="nowrap"><code>[HTML], [S5], or [RTF]?</code></span></td><td style="text-align: left;"><span class="nowrap"><code>html-s5-or-rtf</code></span></td></tr><tr class="odd"><td style="text-align: left;"><span class="nowrap"><code>3. Applications</code></span></td><td style="text-align: left;"><span class="nowrap"><code>applications</code></span></td></tr><tr class="even"><td style="text-align: left;"><span class="nowrap"><code>33</code></span></td><td style="text-align: left;"><span class="nowrap"><code>section</code></span></td></tr></tbody></table>

These rules should, in most cases, allow one to determine the identifier from the heading text. The exception is when several headings have the same text; in this case, the first will get an identifier as described above; the second will get the same identifier with <span class="nowrap">`-1`</span> appended; the third with <span class="nowrap">`-2`</span>; and so on.

(However, a different algorithm is used if <span class="nowrap">`gfm_auto_identifiers`</span> is enabled; see below.)

These identifiers are used to provide link targets in the table of contents generated by the <a href="#option--toc%5B" class="option"><span class="nowrap"><code>--toc|--table-of-contents</code></span></a> option. They also make it easy to provide links from one section of a document to another. A link to this section, for example, might look like this:

    See the section on
    [heading identifiers](#heading-identifiers-in-html-latex-and-context).

Note, however, that this method of providing links to sections works only in HTML, LaTeX, and ConTeXt formats.

If the <a href="#option--section-divs%5B" class="option"><span class="nowrap"><code>--section-divs</code></span></a> option is specified, then each section will be wrapped in a <span class="nowrap">`section`</span> (or a <span class="nowrap">`div`</span>, if <span class="nowrap">`html4`</span> was specified), and the identifier will be attached to the enclosing <span class="nowrap">`<section>`</span> (or <span class="nowrap">`<div>`</span>) tag rather than the heading itself. This allows entire sections to be manipulated using JavaScript or treated differently in CSS.

### <a href="#extension-ascii_identifiers" class="anchor"></a>Extension: <span class="nowrap">`ascii_identifiers`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Causes the identifiers produced by <span class="nowrap">`auto_identifiers`</span> to be pure ASCII. Accents are stripped off of accented Latin letters, and non-Latin letters are omitted.

### <a href="#extension-gfm_auto_identifiers" class="anchor"></a>Extension: <span class="nowrap">`gfm_auto_identifiers`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Changes the algorithm used by <span class="nowrap">`auto_identifiers`</span> to conform to GitHub’s method. Spaces are converted to dashes (<span class="nowrap">`-`</span>), uppercase characters to lowercase characters, and punctuation characters other than <span class="nowrap">`-`</span> and <span class="nowrap">`_`</span> are removed. Emojis are replaced by their names.

<a href="#math-input" class="anchor"></a>Math Input <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------

The extensions [<span class="nowrap">`tex_math_dollars`</span>](#extension-tex_math_dollars), [<span class="nowrap">`tex_math_gfm`</span>](#extension-tex_math_gfm), [<span class="nowrap">`tex_math_single_backslash`</span>](#extension-tex_math_single_backslash), and [<span class="nowrap">`tex_math_double_backslash`</span>](#extension-tex_math_double_backslash) are described in the section about Pandoc’s Markdown.

However, they can also be used with HTML input. This is handy for reading web pages formatted using MathJax, for example.

<a href="#raw-htmltex" class="anchor"></a>Raw HTML/TeX <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
----------------------------------------------------------------------------------------------------------------------

The following extensions are described in more detail in their respective sections of [Pandoc’s Markdown](#pandocs-markdown):

-   [<span class="nowrap">`raw_html`</span>](#extension-raw_html) allows HTML elements which are not representable in pandoc’s AST to be parsed as raw HTML. By default, this is disabled for HTML input.

-   [<span class="nowrap">`raw_tex`</span>](#extension-raw_tex) allows raw LaTeX, TeX, and ConTeXt to be included in a document. This extension can be enabled/disabled for the following formats (in addition to <span class="nowrap">`markdown`</span>):

    input formats  
    <span class="nowrap">`latex`</span>, <span class="nowrap">`textile`</span>, <span class="nowrap">`html`</span> (environments, <span class="nowrap">`\ref`</span>, and <span class="nowrap">`\eqref`</span> only), <span class="nowrap">`ipynb`</span>

    output formats  
    <span class="nowrap">`textile`</span>, <span class="nowrap">`commonmark`</span>

    Note: as applied to <span class="nowrap">`ipynb`</span>, <span class="nowrap">`raw_html`</span> and <span class="nowrap">`raw_tex`</span> affect not only raw TeX in Markdown cells, but data with mime type <span class="nowrap">`text/html`</span> in output cells. Since the <span class="nowrap">`ipynb`</span> reader attempts to preserve the richest possible outputs when several options are given, you will get best results if you disable <span class="nowrap">`raw_html`</span> and <span class="nowrap">`raw_tex`</span> when converting to formats like <span class="nowrap">`docx`</span> which don’t allow raw <span class="nowrap">`html`</span> or <span class="nowrap">`tex`</span>.

-   [<span class="nowrap">`native_divs`</span>](#extension-native_divs) causes HTML <span class="nowrap">`div`</span> elements to be parsed as native pandoc Div blocks. If you want them to be parsed as raw HTML, use <a href="#option--from" class="option"><span class="nowrap"><code>-f html-native_divs+raw_html</code></span></a>.

-   [<span class="nowrap">`native_spans`</span>](#extension-native_spans) causes HTML <span class="nowrap">`span`</span> elements to be parsed as native pandoc Span inlines. If you want them to be parsed as raw HTML, use <a href="#option--from" class="option"><span class="nowrap"><code>-f html-native_spans+raw_html</code></span></a>. If you want to drop all <span class="nowrap">`div`</span>s and <span class="nowrap">`span`</span>s when converting HTML to Markdown, you can use <span class="nowrap">`pandoc -f html-native_divs-native_spans -t markdown`</span>.

<a href="#literate-haskell-support" class="anchor"></a>Literate Haskell support <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------------------------

### <a href="#extension-literate_haskell" class="anchor"></a>Extension: <span class="nowrap">`literate_haskell`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Treat the document as literate Haskell source.

This extension can be enabled/disabled for the following formats:

input formats  
<span class="nowrap">`markdown`</span>, <span class="nowrap">`rst`</span>, <span class="nowrap">`latex`</span>

output formats  
<span class="nowrap">`markdown`</span>, <span class="nowrap">`rst`</span>, <span class="nowrap">`latex`</span>, <span class="nowrap">`html`</span>

If you append <span class="nowrap">`+lhs`</span> (or <span class="nowrap">`+literate_haskell`</span>) to one of the formats above, pandoc will treat the document as literate Haskell source. This means that

-   In Markdown input, “bird track” sections will be parsed as Haskell code rather than block quotations. Text between <span class="nowrap">`\begin{code}`</span> and <span class="nowrap">`\end{code}`</span> will also be treated as Haskell code. For ATX-style headings the character ‘=’ will be used instead of ‘\#’.

-   In Markdown output, code blocks with classes <span class="nowrap">`haskell`</span> and <span class="nowrap">`literate`</span> will be rendered using bird tracks, and block quotations will be indented one space, so they will not be treated as Haskell code. In addition, headings will be rendered setext-style (with underlines) rather than ATX-style (with ‘\#’ characters). (This is because ghc treats ‘\#’ characters in column 1 as introducing line numbers.)

-   In restructured text input, “bird track” sections will be parsed as Haskell code.

-   In restructured text output, code blocks with class <span class="nowrap">`haskell`</span> will be rendered using bird tracks.

-   In LaTeX input, text in <span class="nowrap">`code`</span> environments will be parsed as Haskell code.

-   In LaTeX output, code blocks with class <span class="nowrap">`haskell`</span> will be rendered inside <span class="nowrap">`code`</span> environments.

-   In HTML output, code blocks with class <span class="nowrap">`haskell`</span> will be rendered with class <span class="nowrap">`literatehaskell`</span> and bird tracks.

Examples:

    pandoc -f markdown+lhs -t html

reads literate Haskell source formatted with Markdown conventions and writes ordinary HTML (without bird tracks).

    pandoc -f markdown+lhs -t html+lhs

writes HTML with the Haskell code in bird tracks, so it can be copied and pasted as literate Haskell source.

Note that GHC expects the bird tracks in the first column, so indented literate code blocks (e.g. inside an itemized environment) will not be picked up by the Haskell compiler.

<a href="#other-extensions" class="anchor"></a>Other extensions <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------------

### <a href="#extension-empty_paragraphs" class="anchor"></a>Extension: <span class="nowrap">`empty_paragraphs`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Allows empty paragraphs. By default empty paragraphs are omitted.

This extension can be enabled/disabled for the following formats:

input formats  
<span class="nowrap">`docx`</span>, <span class="nowrap">`html`</span>

output formats  
<span class="nowrap">`docx`</span>, <span class="nowrap">`odt`</span>, <span class="nowrap">`opendocument`</span>, <span class="nowrap">`html`</span>, <span class="nowrap">`latex`</span>

### <a href="#extension-native_numbering" class="anchor"></a>Extension: <span class="nowrap">`native_numbering`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Enables native numbering of figures and tables. Enumeration starts at 1.

This extension can be enabled/disabled for the following formats:

output formats  
<span class="nowrap">`odt`</span>, <span class="nowrap">`opendocument`</span>, <span class="nowrap">`docx`</span>

### <a href="#extension-xrefs_name" class="anchor"></a>Extension: <span class="nowrap">`xrefs_name`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Links to headings, figures and tables inside the document are substituted with cross-references that will use the name or caption of the referenced item. The original link text is replaced once the generated document is refreshed. This extension can be combined with <span class="nowrap">`xrefs_number`</span> in which case numbers will appear before the name.

Text in cross-references is only made consistent with the referenced item once the document has been refreshed.

This extension can be enabled/disabled for the following formats:

output formats  
<span class="nowrap">`odt`</span>, <span class="nowrap">`opendocument`</span>

### <a href="#extension-xrefs_number" class="anchor"></a>Extension: <span class="nowrap">`xrefs_number`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Links to headings, figures and tables inside the document are substituted with cross-references that will use the number of the referenced item. The original link text is discarded. This extension can be combined with <span class="nowrap">`xrefs_name`</span> in which case the name or caption numbers will appear after the number.

For the <span class="nowrap">`xrefs_number`</span> to be useful heading numbers must be enabled in the generated document, also table and figure captions must be enabled using for example the <span class="nowrap">`native_numbering`</span> extension.

Numbers in cross-references are only visible in the final document once it has been refreshed.

This extension can be enabled/disabled for the following formats:

output formats  
<span class="nowrap">`odt`</span>, <span class="nowrap">`opendocument`</span>

### <a href="#ext-styles" class="anchor"></a>Extension: <span class="nowrap">`styles`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

When converting from docx, read all docx styles as divs (for paragraph styles) and spans (for character styles) regardless of whether pandoc understands the meaning of these styles. This can be used with [docx custom styles](#custom-styles). Disabled by default.

input formats  
<span class="nowrap">`docx`</span>

### <a href="#extension-amuse" class="anchor"></a>Extension: <span class="nowrap">`amuse`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

In the <span class="nowrap">`muse`</span> input format, this enables Text::Amuse extensions to Emacs Muse markup.

### <a href="#extension-raw_markdown" class="anchor"></a>Extension: <span class="nowrap">`raw_markdown`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

In the <span class="nowrap">`ipynb`</span> input format, this causes Markdown cells to be included as raw Markdown blocks (allowing lossless round-tripping) rather than being parsed. Use this only when you are targeting <span class="nowrap">`ipynb`</span> or a Markdown-based output format.

### <a href="#typst-citations" class="anchor"></a>Extension: <span class="nowrap">`citations`</span> (typst) <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

When the <span class="nowrap">`citations`</span> extension is enabled in <span class="nowrap">`typst`</span> (as it is by default), <span class="nowrap">`typst`</span> citations will be parsed as native pandoc citations, and native pandoc citations will be rendered as <span class="nowrap">`typst`</span> citations.

### <a href="#org-citations" class="anchor"></a>Extension: <span class="nowrap">`citations`</span> (org) <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

When the <span class="nowrap">`citations`</span> extension is enabled in <span class="nowrap">`org`</span>, org-cite and org-ref style citations will be parsed as native pandoc citations, and org-cite citations will be used to render native pandoc citations.

### <a href="#docx-citations" class="anchor"></a>Extension: <span class="nowrap">`citations`</span> (docx) <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

When <span class="nowrap">`citations`</span> is enabled in <span class="nowrap">`docx`</span>, citations inserted by Zotero or Mendeley or EndNote plugins will be parsed as native pandoc citations. (Otherwise, the formatted citations generated by the bibliographic software will be parsed as regular text.)

### <a href="#org-fancy-lists" class="anchor"></a>Extension: <span class="nowrap">`fancy_lists`</span> (org) <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Some aspects of [Pandoc’s Markdown fancy lists](#extension-fancy_lists) are also accepted in <span class="nowrap">`org`</span> input, mimicking the option <span class="nowrap">`org-list-allow-alphabetical`</span> in Emacs. As in Org Mode, enabling this extension allows lowercase and uppercase alphabetical markers for ordered lists to be parsed in addition to arabic ones. Note that for Org, this does not include roman numerals or the <span class="nowrap">`#`</span> placeholder that are enabled by the extension in Pandoc’s Markdown.

### <a href="#extension-element_citations" class="anchor"></a>Extension: <span class="nowrap">`element_citations`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

In the <span class="nowrap">`jats`</span> output formats, this causes reference items to be replaced with <span class="nowrap">`<element-citation>`</span> elements. These elements are not influenced by CSL styles, but all information on the item is included in tags.

### <a href="#extension-ntb" class="anchor"></a>Extension: <span class="nowrap">`ntb`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

In the <span class="nowrap">`context`</span> output format this enables the use of [Natural Tables (TABLE)](https://wiki.contextgarden.net/TABLE) instead of the default [Extreme Tables (xtables)](https://wiki.contextgarden.net/xtables). Natural tables allow more fine-grained global customization but come at a performance penalty compared to extreme tables.

### <a href="#extension--tagging" class="anchor"></a>Extension: <span class="nowrap">`tagging`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Enabling this extension with <span class="nowrap">`context`</span> output will produce markup suitable for the production of tagged PDFs. This includes additional markers for paragraphs and alternative markup for emphasized text. The <span class="nowrap">`emphasis-command`</span> template variable is set if the extension is enabled.

<a href="#pandocs-markdown" class="anchor"></a>Pandoc’s Markdown <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
================================================================================================================================

Pandoc understands an extended and slightly revised version of John Gruber’s [Markdown](https://daringfireball.net/projects/markdown/) syntax. This document explains the syntax, noting differences from original Markdown. Except where noted, these differences can be suppressed by using the <span class="nowrap">`markdown_strict`</span> format instead of <span class="nowrap">`markdown`</span>. Extensions can be enabled or disabled to specify the behavior more granularly. They are described in the following. See also [Extensions](#extensions) above, for extensions that work also on other formats.

<a href="#philosophy" class="anchor"></a>Philosophy <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------

Markdown is designed to be easy to write, and, even more importantly, easy to read:

> A Markdown-formatted document should be publishable as-is, as plain text, without looking like it’s been marked up with tags or formatting instructions.  
> – [John Gruber](https://daringfireball.net/projects/markdown/syntax#philosophy)

This principle has guided pandoc’s decisions in finding syntax for tables, footnotes, and other extensions.

There is, however, one respect in which pandoc’s aims are different from the original aims of Markdown. Whereas Markdown was originally designed with HTML generation in mind, pandoc is designed for multiple output formats. Thus, while pandoc allows the embedding of raw HTML, it discourages it, and provides other, non-HTMLish ways of representing important document elements like definition lists, tables, mathematics, and footnotes.

<a href="#paragraphs" class="anchor"></a>Paragraphs <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------

A paragraph is one or more lines of text followed by one or more blank lines. Newlines are treated as spaces, so you can reflow your paragraphs as you like. If you need a hard line break, put two or more spaces at the end of a line.

### <a href="#extension-escaped_line_breaks" class="anchor"></a>Extension: <span class="nowrap">`escaped_line_breaks`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A backslash followed by a newline is also a hard line break. Note: in multiline and grid table cells, this is the only way to create a hard line break, since trailing spaces in the cells are ignored.

<a href="#headings" class="anchor"></a>Headings <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------

There are two kinds of headings: Setext and ATX.

### <a href="#setext-style-headings" class="anchor"></a>Setext-style headings <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A setext-style heading is a line of text “underlined” with a row of <span class="nowrap">`=`</span> signs (for a level-one heading) or <span class="nowrap">`-`</span> signs (for a level-two heading):

    A level-one heading
    ===================

    A level-two heading
    -------------------

The heading text can contain inline formatting, such as emphasis (see [Inline formatting](#inline-formatting), below).

### <a href="#atx-style-headings" class="anchor"></a>ATX-style headings <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

An ATX-style heading consists of one to six <span class="nowrap">`#`</span> signs and a line of text, optionally followed by any number of <span class="nowrap">`#`</span> signs. The number of <span class="nowrap">`#`</span> signs at the beginning of the line is the heading level:

    ## A level-two heading

    ### A level-three heading ###

As with setext-style headings, the heading text can contain formatting:

    # A level-one heading with a [link](/url) and *emphasis*

### <a href="#extension-blank_before_header" class="anchor"></a>Extension: <span class="nowrap">`blank_before_header`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Original Markdown syntax does not require a blank line before a heading. Pandoc does require this (except, of course, at the beginning of the document). The reason for the requirement is that it is all too easy for a <span class="nowrap">`#`</span> to end up at the beginning of a line by accident (perhaps through line wrapping). Consider, for example:

    I like several of their flavors of ice cream:
    #22, for example, and #5.

### <a href="#extension-space_in_atx_header" class="anchor"></a>Extension: <span class="nowrap">`space_in_atx_header`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Many Markdown implementations do not require a space between the opening <span class="nowrap">`#`</span>s of an ATX heading and the heading text, so that <span class="nowrap">`#5 bolt`</span> and <span class="nowrap">`#hashtag`</span> count as headings. With this extension, pandoc does require the space.

### <a href="#heading-identifiers" class="anchor"></a>Heading identifiers <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

See also the [<span class="nowrap">`auto_identifiers`</span> extension](#extension-auto_identifiers) above.

### <a href="#extension-header_attributes" class="anchor"></a>Extension: <span class="nowrap">`header_attributes`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Headings can be assigned attributes using this syntax at the end of the line containing the heading text:

    {#identifier .class .class key=value key=value}

Thus, for example, the following headings will all be assigned the identifier <span class="nowrap">`foo`</span>:

    # My heading {#foo}

    ## My heading ##    {#foo}

    My other heading   {#foo}
    ---------------

(This syntax is compatible with [PHP Markdown Extra](https://michelf.ca/projects/php-markdown/extra/).)

Note that although this syntax allows assignment of classes and key/value attributes, writers generally don’t use all of this information. Identifiers, classes, and key/value attributes are used in HTML and HTML-based formats such as EPUB and slidy. Identifiers are used for labels and link anchors in the LaTeX, ConTeXt, Textile, Jira markup, and AsciiDoc writers.

Headings with the class <span class="nowrap">`unnumbered`</span> will not be numbered, even if <a href="#option--number-sections" class="option"><span class="nowrap"><code>--number-sections</code></span></a> is specified. A single hyphen (<span class="nowrap">`-`</span>) in an attribute context is equivalent to <span class="nowrap">`.unnumbered`</span>, and preferable in non-English documents. So,

    # My heading {-}

is just the same as

    # My heading {.unnumbered}

If the <span class="nowrap">`unlisted`</span> class is present in addition to <span class="nowrap">`unnumbered`</span>, the heading will not be included in a table of contents. (Currently this feature is only implemented for certain formats: those based on LaTeX and HTML, PowerPoint, and RTF.)

### <a href="#extension-implicit_header_references" class="anchor"></a>Extension: <span class="nowrap">`implicit_header_references`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Pandoc behaves as if reference links have been defined for each heading. So, to link to a heading

    # Heading identifiers in HTML

you can simply write

    [Heading identifiers in HTML]

or

    [Heading identifiers in HTML][]

or

    [the section on heading identifiers][heading identifiers in
    HTML]

instead of giving the identifier explicitly:

    [Heading identifiers in HTML](#heading-identifiers-in-html)

If there are multiple headings with identical text, the corresponding reference will link to the first one only, and you will need to use explicit links to link to the others, as described above.

Like regular reference links, these references are case-insensitive.

Explicit link reference definitions always take priority over implicit heading references. So, in the following example, the link will point to <span class="nowrap">`bar`</span>, not to <span class="nowrap">`#foo`</span>:

    # Foo

    [foo]: bar

    See [foo]

<a href="#block-quotations" class="anchor"></a>Block quotations <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------------

Markdown uses email conventions for quoting blocks of text. A block quotation is one or more paragraphs or other block elements (such as lists or headings), with each line preceded by a <span class="nowrap">`>`</span> character and an optional space. (The <span class="nowrap">`>`</span> need not start at the left margin, but it should not be indented more than three spaces.)

    > This is a block quote. This
    > paragraph has two lines.
    >
    > 1. This is a list inside a block quote.
    > 2. Second item.

A “lazy” form, which requires the <span class="nowrap">`>`</span> character only on the first line of each block, is also allowed:

    > This is a block quote. This
    paragraph has two lines.

    > 1. This is a list inside a block quote.
    2. Second item.

Among the block elements that can be contained in a block quote are other block quotes. That is, block quotes can be nested:

    > This is a block quote.
    >
    > > A block quote within a block quote.

If the <span class="nowrap">`>`</span> character is followed by an optional space, that space will be considered part of the block quote marker and not part of the indentation of the contents. Thus, to put an indented code block in a block quote, you need five spaces after the <span class="nowrap">`>`</span>:

    >     code

### <a href="#extension-blank_before_blockquote" class="anchor"></a>Extension: <span class="nowrap">`blank_before_blockquote`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Original Markdown syntax does not require a blank line before a block quote. Pandoc does require this (except, of course, at the beginning of the document). The reason for the requirement is that it is all too easy for a <span class="nowrap">`>`</span> to end up at the beginning of a line by accident (perhaps through line wrapping). So, unless the <span class="nowrap">`markdown_strict`</span> format is used, the following does not produce a nested block quote in pandoc:

    > This is a block quote.
    >> Not nested, since `blank_before_blockquote` is enabled by default

<a href="#verbatim-code-blocks" class="anchor"></a>Verbatim (code) blocks <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------------------

### <a href="#indented-code-blocks" class="anchor"></a>Indented code blocks <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A block of text indented four spaces (or one tab) is treated as verbatim text: that is, special characters do not trigger special formatting, and all spaces and line breaks are preserved. For example,

        if (a > 3) {
          moveShip(5 * gravity, DOWN);
        }

The initial (four space or one tab) indentation is not considered part of the verbatim text, and is removed in the output.

Note: blank lines in the verbatim text need not begin with four spaces.

### <a href="#fenced-code-blocks" class="anchor"></a>Fenced code blocks <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

### <a href="#extension-fenced_code_blocks" class="anchor"></a>Extension: <span class="nowrap">`fenced_code_blocks`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

In addition to standard indented code blocks, pandoc supports *fenced* code blocks. These begin with a row of three or more tildes (<span class="nowrap">`~`</span>) and end with a row of tildes that must be at least as long as the starting row. Everything between these lines is treated as code. No indentation is necessary:

    ~~~~~~~
    if (a > 3) {
      moveShip(5 * gravity, DOWN);
    }
    ~~~~~~~

Like regular code blocks, fenced code blocks must be separated from surrounding text by blank lines.

If the code itself contains a row of tildes or backticks, just use a longer row of tildes or backticks at the start and end:

    ~~~~~~~~~~~~~~~~
    ~~~~~~~~~~
    code including tildes
    ~~~~~~~~~~
    ~~~~~~~~~~~~~~~~

### <a href="#extension-backtick_code_blocks" class="anchor"></a>Extension: <span class="nowrap">`backtick_code_blocks`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Same as <span class="nowrap">`fenced_code_blocks`</span>, but uses backticks (<span class="nowrap">`` ` ``</span>) instead of tildes (<span class="nowrap">`~`</span>).

### <a href="#extension-fenced_code_attributes" class="anchor"></a>Extension: <span class="nowrap">`fenced_code_attributes`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Optionally, you may attach attributes to fenced or backtick code block using this syntax:

    ~~~~ {#mycode .haskell .numberLines startFrom="100"}
    qsort []     = []
    qsort (x:xs) = qsort (filter (< x) xs) ++ [x] ++
                   qsort (filter (>= x) xs)
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Here <span class="nowrap">`mycode`</span> is an identifier, <span class="nowrap">`haskell`</span> and <span class="nowrap">`numberLines`</span> are classes, and <span class="nowrap">`startFrom`</span> is an attribute with value <span class="nowrap">`100`</span>. Some output formats can use this information to do syntax highlighting. Currently, the only output formats that use this information are HTML, LaTeX, Docx, Ms, and PowerPoint. If highlighting is supported for your output format and language, then the code block above will appear highlighted, with numbered lines. (To see which languages are supported, type <span class="nowrap">`pandoc --list-highlight-languages`</span>.) Otherwise, the code block above will appear as follows:

    <pre id="mycode" class="haskell numberLines" startFrom="100">
      <code>
      ...
      </code>
    </pre>

The <span class="nowrap">`numberLines`</span> (or <span class="nowrap">`number-lines`</span>) class will cause the lines of the code block to be numbered, starting with <span class="nowrap">`1`</span> or the value of the <span class="nowrap">`startFrom`</span> attribute. The <span class="nowrap">`lineAnchors`</span> (or <span class="nowrap">`line-anchors`</span>) class will cause the lines to be clickable anchors in HTML output.

A shortcut form can also be used for specifying the language of the code block:

    ```haskell
    qsort [] = []
    ```

This is equivalent to:

    ``` {.haskell}
    qsort [] = []
    ```

This shortcut form may be combined with attributes:

    ```haskell {.numberLines}
    qsort [] = []
    ```

Which is equivalent to:

    ``` {.haskell .numberLines}
    qsort [] = []
    ```

If the <span class="nowrap">`fenced_code_attributes`</span> extension is disabled, but input contains class attribute(s) for the code block, the first class attribute will be printed after the opening fence as a bare word.

To prevent all highlighting, use the <a href="#option--no-highlight" class="option"><span class="nowrap"><code>--no-highlight</code></span></a> flag. To set the highlighting style, use <a href="#option--highlight-style" class="option"><span class="nowrap"><code>--highlight-style</code></span></a>. For more information on highlighting, see [Syntax highlighting](#syntax-highlighting), below.

<a href="#line-blocks" class="anchor"></a>Line blocks <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------

### <a href="#extension-line_blocks" class="anchor"></a>Extension: <span class="nowrap">`line_blocks`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A line block is a sequence of lines beginning with a vertical bar (<span class="nowrap">`|`</span>) followed by a space. The division into lines will be preserved in the output, as will any leading spaces; otherwise, the lines will be formatted as Markdown. This is useful for verse and addresses:

    | The limerick packs laughs anatomical
    | In space that is quite economical.
    |    But the good ones I've seen
    |    So seldom are clean
    | And the clean ones so seldom are comical

    | 200 Main St.
    | Berkeley, CA 94718

The lines can be hard-wrapped if needed, but the continuation line must begin with a space.

    | The Right Honorable Most Venerable and Righteous Samuel L.
      Constable, Jr.
    | 200 Main St.
    | Berkeley, CA 94718

Inline formatting (such as emphasis) is allowed in the content (though it can’t cross line boundaries). Block-level formatting (such as block quotes or lists) is not recognized.

This syntax is borrowed from [reStructuredText](https://docutils.sourceforge.io/docs/ref/rst/introduction.html).

<a href="#lists" class="anchor"></a>Lists <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------

### <a href="#bullet-lists" class="anchor"></a>Bullet lists <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A bullet list is a list of bulleted list items. A bulleted list item begins with a bullet (<span class="nowrap">`*`</span>, <span class="nowrap">`+`</span>, or <span class="nowrap">`-`</span>). Here is a simple example:

    * one
    * two
    * three

This will produce a “compact” list. If you want a “loose” list, in which each item is formatted as a paragraph, put spaces between the items:

    * one

    * two

    * three

The bullets need not be flush with the left margin; they may be indented one, two, or three spaces. The bullet must be followed by whitespace.

List items look best if subsequent lines are flush with the first line (after the bullet):

    * here is my first
      list item.
    * and my second.

But Markdown also allows a “lazy” format:

    * here is my first
    list item.
    * and my second.

### <a href="#block-content-in-list-items" class="anchor"></a>Block content in list items <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A list item may contain multiple paragraphs and other block-level content. However, subsequent paragraphs must be preceded by a blank line and indented to line up with the first non-space content after the list marker.

      * First paragraph.

        Continued.

      * Second paragraph. With a code block, which must be indented
        eight spaces:

            { code }

Exception: if the list marker is followed by an indented code block, which must begin 5 spaces after the list marker, then subsequent paragraphs must begin two columns after the last character of the list marker:

    *     code

      continuation paragraph

List items may include other lists. In this case the preceding blank line is optional. The nested list must be indented to line up with the first non-space character after the list marker of the containing list item.

    * fruits
      + apples
        - macintosh
        - red delicious
      + pears
      + peaches
    * vegetables
      + broccoli
      + chard

As noted above, Markdown allows you to write list items “lazily,” instead of indenting continuation lines. However, if there are multiple paragraphs or other blocks in a list item, the first line of each must be indented.

    + A lazy, lazy, list
    item.

    + Another one; this looks
    bad but is legal.

        Second paragraph of second
    list item.

### <a href="#ordered-lists" class="anchor"></a>Ordered lists <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Ordered lists work just like bulleted lists, except that the items begin with enumerators rather than bullets.

In original Markdown, enumerators are decimal numbers followed by a period and a space. The numbers themselves are ignored, so there is no difference between this list:

    1.  one
    2.  two
    3.  three

and this one:

    5.  one
    7.  two
    1.  three

### <a href="#extension-fancy_lists" class="anchor"></a>Extension: <span class="nowrap">`fancy_lists`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Unlike original Markdown, pandoc allows ordered list items to be marked with uppercase and lowercase letters and roman numerals, in addition to Arabic numerals. List markers may be enclosed in parentheses or followed by a single right-parenthesis or period. They must be separated from the text that follows by at least one space, and, if the list marker is a capital letter with a period, by at least two spaces.<a href="#fn1" id="fnref1" class="footnote-ref"><sup>1</sup></a>

The <span class="nowrap">`fancy_lists`</span> extension also allows ‘<span class="nowrap">`#`</span>’ to be used as an ordered list marker in place of a numeral:

    #. one
    #. two

Note: the ‘<span class="nowrap">`#`</span>’ ordered list marker doesn’t work with <span class="nowrap">`commonmark`</span>.

### <a href="#extension-startnum" class="anchor"></a>Extension: <span class="nowrap">`startnum`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Pandoc also pays attention to the type of list marker used, and to the starting number, and both of these are preserved where possible in the output format. Thus, the following yields a list with numbers followed by a single parenthesis, starting with 9, and a sublist with lowercase roman numerals:

     9)  Ninth
    10)  Tenth
    11)  Eleventh
           i. subone
          ii. subtwo
         iii. subthree

Pandoc will start a new list each time a different type of list marker is used. So, the following will create three lists:

    (2) Two
    (5) Three
    1.  Four
    *   Five

If default list markers are desired, use <span class="nowrap">`#.`</span>:

    #.  one
    #.  two
    #.  three

### <a href="#extension-task_lists" class="anchor"></a>Extension: <span class="nowrap">`task_lists`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Pandoc supports task lists, using the syntax of GitHub-Flavored Markdown.

    - [ ] an unchecked task list item
    - [x] checked item

### <a href="#definition-lists" class="anchor"></a>Definition lists <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

### <a href="#extension-definition_lists" class="anchor"></a>Extension: <span class="nowrap">`definition_lists`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Pandoc supports definition lists, using the syntax of [PHP Markdown Extra](https://michelf.ca/projects/php-markdown/extra/) with some extensions.<a href="#fn2" id="fnref2" class="footnote-ref"><sup>2</sup></a>

    Term 1

    :   Definition 1

    Term 2 with *inline markup*

    :   Definition 2

            { some code, part of Definition 2 }

        Third paragraph of definition 2.

Each term must fit on one line, which may optionally be followed by a blank line, and must be followed by one or more definitions. A definition begins with a colon or tilde, which may be indented one or two spaces.

A term may have multiple definitions, and each definition may consist of one or more block elements (paragraph, code block, list, etc.), each indented four spaces or one tab stop. The body of the definition (not including the first line) should be indented four spaces. However, as with other Markdown lists, you can “lazily” omit indentation except at the beginning of a paragraph or other block element:

    Term 1

    :   Definition
    with lazy continuation.

        Second paragraph of the definition.

If you leave space before the definition (as in the example above), the text of the definition will be treated as a paragraph. In some output formats, this will mean greater spacing between term/definition pairs. For a more compact definition list, omit the space before the definition:

    Term 1
      ~ Definition 1

    Term 2
      ~ Definition 2a
      ~ Definition 2b

Note that space between items in a definition list is required. (A variant that loosens this requirement, but disallows “lazy” hard wrapping, can be activated with the [<span class="nowrap">`compact_definition_lists`</span> extension](#extension-compact_definition_lists).)

### <a href="#numbered-example-lists" class="anchor"></a>Numbered example lists <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

### <a href="#extension-example_lists" class="anchor"></a>Extension: <span class="nowrap">`example_lists`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

The special list marker <span class="nowrap">`@`</span> can be used for sequentially numbered examples. The first list item with a <span class="nowrap">`@`</span> marker will be numbered ‘1’, the next ‘2’, and so on, throughout the document. The numbered examples need not occur in a single list; each new list using <span class="nowrap">`@`</span> will take up where the last stopped. So, for example:

    (@)  My first example will be numbered (1).
    (@)  My second example will be numbered (2).

    Explanation of examples.

    (@)  My third example will be numbered (3).

Numbered examples can be labeled and referred to elsewhere in the document:

    (@good)  This is a good example.

    As (@good) illustrates, ...

The label can be any string of alphanumeric characters, underscores, or hyphens.

Continuation paragraphs in example lists must always be indented four spaces, regardless of the length of the list marker. That is, example lists always behave as if the <span class="nowrap">`four_space_rule`</span> extension is set. This is because example labels tend to be long, and indenting content to the first non-space character after the label would be awkward.

You can repeat an earlier numbered example by re-using its label:

    (@foo) Sample sentence.

    Intervening text...

    This theory can explain the case we saw earlier (repeated):

    (@foo) Sample sentence.

This only works reliably, though, if the repeated item is in a list by itself, because each numbered example list will be numbered continuously from its starting number.

### <a href="#ending-a-list" class="anchor"></a>Ending a list <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

What if you want to put an indented code block after a list?

    -   item one
    -   item two

        { my code block }

Trouble! Here pandoc (like other Markdown implementations) will treat <span class="nowrap">`{ my code block }`</span> as the second paragraph of item two, and not as a code block.

To “cut off” the list after item two, you can insert some non-indented content, like an HTML comment, which won’t produce visible output in any format:

    -   item one
    -   item two

    <!-- end of list -->

        { my code block }

You can use the same trick if you want two consecutive lists instead of one big list:

    1.  one
    2.  two
    3.  three

    <!-- -->

    1.  uno
    2.  dos
    3.  tres

<a href="#horizontal-rules" class="anchor"></a>Horizontal rules <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------------

A line containing a row of three or more <span class="nowrap">`*`</span>, <span class="nowrap">`-`</span>, or <span class="nowrap">`_`</span> characters (optionally separated by spaces) produces a horizontal rule:

    *  *  *  *

    ---------------

We strongly recommend that horizontal rules be separated from surrounding text by blank lines. If a horizontal rule is not followed by a blank line, pandoc may try to interpret the lines that follow as a YAML metadata block or a table.

<a href="#tables" class="anchor"></a>Tables <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------

Four kinds of tables may be used. The first three kinds presuppose the use of a fixed-width font, such as Courier. The fourth kind can be used with proportionally spaced fonts, as it does not require lining up columns.

### <a href="#extension-table_captions" class="anchor"></a>Extension: <span class="nowrap">`table_captions`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A caption may optionally be provided with all 4 kinds of tables (as illustrated in the examples below). A caption is a paragraph beginning with the string <span class="nowrap">`Table:`</span> (or <span class="nowrap">`table:`</span> or just <span class="nowrap">`:`</span>), which will be stripped off. It may appear either before or after the table.

### <a href="#extension-simple_tables" class="anchor"></a>Extension: <span class="nowrap">`simple_tables`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Simple tables look like this:

      Right     Left     Center     Default
    -------     ------ ----------   -------
         12     12        12            12
        123     123       123          123
          1     1          1             1

    Table:  Demonstration of simple table syntax.

The header and table rows must each fit on one line. Column alignments are determined by the position of the header text relative to the dashed line below it:<a href="#fn3" id="fnref3" class="footnote-ref"><sup>3</sup></a>

-   If the dashed line is flush with the header text on the right side but extends beyond it on the left, the column is right-aligned.
-   If the dashed line is flush with the header text on the left side but extends beyond it on the right, the column is left-aligned.
-   If the dashed line extends beyond the header text on both sides, the column is centered.
-   If the dashed line is flush with the header text on both sides, the default alignment is used (in most cases, this will be left).

The table must end with a blank line, or a line of dashes followed by a blank line.

The column header row may be omitted, provided a dashed line is used to end the table. For example:

    -------     ------ ----------   -------
         12     12        12             12
        123     123       123           123
          1     1          1              1
    -------     ------ ----------   -------

When the header row is omitted, column alignments are determined on the basis of the first line of the table body. So, in the tables above, the columns would be right, left, center, and right aligned, respectively.

### <a href="#extension-multiline_tables" class="anchor"></a>Extension: <span class="nowrap">`multiline_tables`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Multiline tables allow header and table rows to span multiple lines of text (but cells that span multiple columns or rows of the table are not supported). Here is an example:

    -------------------------------------------------------------
     Centered   Default           Right Left
      Header    Aligned         Aligned Aligned
    ----------- ------- --------------- -------------------------
       First    row                12.0 Example of a row that
                                        spans multiple lines.

      Second    row                 5.0 Here's another one. Note
                                        the blank line between
                                        rows.
    -------------------------------------------------------------

    Table: Here's the caption. It, too, may span
    multiple lines.

These work like simple tables, but with the following differences:

-   They must begin with a row of dashes, before the header text (unless the header row is omitted).
-   They must end with a row of dashes, then a blank line.
-   The rows must be separated by blank lines.

In multiline tables, the table parser pays attention to the widths of the columns, and the writers try to reproduce these relative widths in the output. So, if you find that one of the columns is too narrow in the output, try widening it in the Markdown source.

The header may be omitted in multiline tables as well as simple tables:

    ----------- ------- --------------- -------------------------
       First    row                12.0 Example of a row that
                                        spans multiple lines.

      Second    row                 5.0 Here's another one. Note
                                        the blank line between
                                        rows.
    ----------- ------- --------------- -------------------------

    : Here's a multiline table without a header.

It is possible for a multiline table to have just one row, but the row should be followed by a blank line (and then the row of dashes that ends the table), or the table may be interpreted as a simple table.

### <a href="#extension-grid_tables" class="anchor"></a>Extension: <span class="nowrap">`grid_tables`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Grid tables look like this:

    : Sample grid table.

    +---------------+---------------+--------------------+
    | Fruit         | Price         | Advantages         |
    +===============+===============+====================+
    | Bananas       | $1.34         | - built-in wrapper |
    |               |               | - bright color     |
    +---------------+---------------+--------------------+
    | Oranges       | $2.10         | - cures scurvy     |
    |               |               | - tasty            |
    +---------------+---------------+--------------------+

The row of <span class="nowrap">`=`</span>s separates the header from the table body, and can be omitted for a headerless table. The cells of grid tables may contain arbitrary block elements (multiple paragraphs, code blocks, lists, etc.).

Cells can span multiple columns or rows:

    +---------------------+----------+
    | Property            | Earth    |
    +=============+=======+==========+
    |             | min   | -89.2 °C |
    | Temperature +-------+----------+
    | 1961-1990   | mean  | 14 °C    |
    |             +-------+----------+
    |             | max   | 56.7 °C  |
    +-------------+-------+----------+

A table header may contain more than one row:

    +---------------------+-----------------------+
    | Location            | Temperature 1961-1990 |
    |                     | in degree Celsius     |
    |                     +-------+-------+-------+
    |                     | min   | mean  | max   |
    +=====================+=======+=======+=======+
    | Antarctica          | -89.2 | N/A   | 19.8  |
    +---------------------+-------+-------+-------+
    | Earth               | -89.2 | 14    | 56.7  |
    +---------------------+-------+-------+-------+

Alignments can be specified as with pipe tables, by putting colons at the boundaries of the separator line after the header:

    +---------------+---------------+--------------------+
    | Right         | Left          | Centered           |
    +==============:+:==============+:==================:+
    | Bananas       | $1.34         | built-in wrapper   |
    +---------------+---------------+--------------------+

For headerless tables, the colons go on the top line instead:

    +--------------:+:--------------+:------------------:+
    | Right         | Left          | Centered           |
    +---------------+---------------+--------------------+

A table foot can be defined by enclosing it with separator lines that use <span class="nowrap">`=`</span> instead of <span class="nowrap">`-`</span>:

     +---------------+---------------+
     | Fruit         | Price         |
     +===============+===============+
     | Bananas       | $1.34         |
     +---------------+---------------+
     | Oranges       | $2.10         |
     +===============+===============+
     | Sum           | $3.44         |
     +===============+===============+

The foot must always be placed at the very bottom of the table.

Grid tables can be created easily using Emacs’ table-mode (<span class="nowrap">`M-x table-insert`</span>).

### <a href="#extension-pipe_tables" class="anchor"></a>Extension: <span class="nowrap">`pipe_tables`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Pipe tables look like this:

    | Right | Left | Default | Center |
    |------:|:-----|---------|:------:|
    |   12  |  12  |    12   |    12  |
    |  123  |  123 |   123   |   123  |
    |    1  |    1 |     1   |     1  |

      : Demonstration of pipe table syntax.

The syntax is identical to [PHP Markdown Extra tables](https://michelf.ca/projects/php-markdown/extra/#table). The beginning and ending pipe characters are optional, but pipes are required between all columns. The colons indicate column alignment as shown. The header cannot be omitted. To simulate a headerless table, include a header with blank cells.

Since the pipes indicate column boundaries, columns need not be vertically aligned, as they are in the above example. So, this is a perfectly legal (though ugly) pipe table:

    fruit| price
    -----|-----:
    apple|2.05
    pear|1.37
    orange|3.09

The cells of pipe tables cannot contain block elements like paragraphs and lists, and cannot span multiple lines. If any line of the Markdown source is longer than the column width (see <a href="#option--columns" class="option"><span class="nowrap"><code>--columns</code></span></a>), then the table will take up the full text width and the cell contents will wrap, with the relative cell widths determined by the number of dashes in the line separating the table header from the table body. (For example <span class="nowrap">`---|-`</span> would make the first column 3/4 and the second column 1/4 of the full text width.) On the other hand, if no lines are wider than column width, then cell contents will not be wrapped, and the cells will be sized to their contents.

Note: pandoc also recognizes pipe tables of the following form, as can be produced by Emacs’ orgtbl-mode:

    | One | Two   |
    |-----+-------|
    | my  | table |
    | is  | nice  |

The difference is that <span class="nowrap">`+`</span> is used instead of <span class="nowrap">`|`</span>. Other orgtbl features are not supported. In particular, to get non-default column alignment, you’ll need to add colons as above.

<a href="#metadata-blocks" class="anchor"></a>Metadata blocks <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------

### <a href="#extension-pandoc_title_block" class="anchor"></a>Extension: <span class="nowrap">`pandoc_title_block`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

If the file begins with a title block

    % title
    % author(s) (separated by semicolons)
    % date

it will be parsed as bibliographic information, not regular text. (It will be used, for example, in the title of standalone LaTeX or HTML output.) The block may contain just a title, a title and an author, or all three elements. If you want to include an author but no title, or a title and a date but no author, you need a blank line:

    %
    % Author

    % My title
    %
    % June 15, 2006

The title may occupy multiple lines, but continuation lines must begin with leading space, thus:

    % My title
      on multiple lines

If a document has multiple authors, the authors may be put on separate lines with leading space, or separated by semicolons, or both. So, all of the following are equivalent:

    % Author One
      Author Two

    % Author One; Author Two

    % Author One;
      Author Two

The date must fit on one line.

All three metadata fields may contain standard inline formatting (italics, links, footnotes, etc.).

Title blocks will always be parsed, but they will affect the output only when the <a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a> (<a href="#option--standalone" class="option"><span class="nowrap"><code>-s</code></span></a>) option is chosen. In HTML output, titles will appear twice: once in the document head—this is the title that will appear at the top of the window in a browser—and once at the beginning of the document body. The title in the document head can have an optional prefix attached (<a href="#option--title-prefix" class="option"><span class="nowrap"><code>--title-prefix</code></span></a> or <a href="#option--title-prefix" class="option"><span class="nowrap"><code>-T</code></span></a> option). The title in the body appears as an H1 element with class “title”, so it can be suppressed or reformatted with CSS. If a title prefix is specified with <a href="#option--title-prefix" class="option"><span class="nowrap"><code>-T</code></span></a> and no title block appears in the document, the title prefix will be used by itself as the HTML title.

The man page writer extracts a title, man page section number, and other header and footer information from the title line. The title is assumed to be the first word on the title line, which may optionally end with a (single-digit) section number in parentheses. (There should be no space between the title and the parentheses.) Anything after this is assumed to be additional footer and header text. A single pipe character (<span class="nowrap">`|`</span>) should be used to separate the footer text from the header text. Thus,

    % PANDOC(1)

will yield a man page with the title <span class="nowrap">`PANDOC`</span> and section 1.

    % PANDOC(1) Pandoc User Manuals

will also have “Pandoc User Manuals” in the footer.

    % PANDOC(1) Pandoc User Manuals | Version 4.0

will also have “Version 4.0” in the header.

### <a href="#extension-yaml_metadata_block" class="anchor"></a>Extension: <span class="nowrap">`yaml_metadata_block`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A [YAML](https://yaml.org/spec/1.2/spec.html "YAML v1.2 Spec") metadata block is a valid YAML object, delimited by a line of three hyphens (<span class="nowrap">`---`</span>) at the top and a line of three hyphens (<span class="nowrap">`---`</span>) or three dots (<span class="nowrap">`...`</span>) at the bottom. The initial line <span class="nowrap">`---`</span> must not be followed by a blank line. A YAML metadata block may occur anywhere in the document, but if it is not at the beginning, it must be preceded by a blank line.

Note that, because of the way pandoc concatenates input files when several are provided, you may also keep the metadata in a separate YAML file and pass it to pandoc as an argument, along with your Markdown files:

    pandoc chap1.md chap2.md chap3.md metadata.yaml -s -o book.html

Just be sure that the YAML file begins with <span class="nowrap">`---`</span> and ends with <span class="nowrap">`---`</span> or <span class="nowrap">`...`</span>. Alternatively, you can use the <a href="#option--metadata-file" class="option"><span class="nowrap"><code>--metadata-file</code></span></a> option. Using that approach however, you cannot reference content (like footnotes) from the main Markdown input document.

Metadata will be taken from the fields of the YAML object and added to any existing document metadata. Metadata can contain lists and objects (nested arbitrarily), but all string scalars will be interpreted as Markdown. Fields with names ending in an underscore will be ignored by pandoc. (They may be given a role by external processors.) Field names must not be interpretable as YAML numbers or boolean values (so, for example, <span class="nowrap">`yes`</span>, <span class="nowrap">`True`</span>, and <span class="nowrap">`15`</span> cannot be used as field names).

A document may contain multiple metadata blocks. If two metadata blocks attempt to set the same field, the value from the second block will be taken.

Each metadata block is handled internally as an independent YAML document. This means, for example, that any YAML anchors defined in a block cannot be referenced in another block.

When pandoc is used with <a href="#option--to" class="option"><span class="nowrap"><code>-t markdown</code></span></a> to create a Markdown document, a YAML metadata block will be produced only if the <a href="#option--standalone" class="option"><span class="nowrap"><code>-s/--standalone</code></span></a> option is used. All of the metadata will appear in a single block at the beginning of the document.

Note that [YAML](https://yaml.org/spec/1.2/spec.html "YAML v1.2 Spec") escaping rules must be followed. Thus, for example, if a title contains a colon, it must be quoted, and if it contains a backslash escape, then it must be ensured that it is not treated as a [YAML escape sequence](https://yaml.org/spec/1.2/spec.html#id2776092). The pipe character (<span class="nowrap">`|`</span>) can be used to begin an indented block that will be interpreted literally, without need for escaping. This form is necessary when the field contains blank lines or block-level formatting:

    ---
    title:  'This is the title: it contains a colon'
    author:
    - Author One
    - Author Two
    keywords: [nothing, nothingness]
    abstract: |
      This is the abstract.

      It consists of two paragraphs.
    ...

The literal block after the <span class="nowrap">`|`</span> must be indented relative to the line containing the <span class="nowrap">`|`</span>. If it is not, the YAML will be invalid and pandoc will not interpret it as metadata. For an overview of the complex rules governing YAML, see the [Wikipedia entry on YAML syntax](https://en.wikipedia.org/wiki/YAML#Syntax).

Template variables will be set automatically from the metadata. Thus, for example, in writing HTML, the variable <span class="nowrap">`abstract`</span> will be set to the HTML equivalent of the Markdown in the <span class="nowrap">`abstract`</span> field:

    <p>This is the abstract.</p>
    <p>It consists of two paragraphs.</p>

Variables can contain arbitrary YAML structures, but the template must match this structure. The <span class="nowrap">`author`</span> variable in the default templates expects a simple list or string, but can be changed to support more complicated structures. The following combination, for example, would add an affiliation to the author if one is given:

    ---
    title: The document title
    author:
    - name: Author One
      affiliation: University of Somewhere
    - name: Author Two
      affiliation: University of Nowhere
    ...

To use the structured authors in the example above, you would need a custom template:

    $for(author)$
    $if(author.name)$
    $author.name$$if(author.affiliation)$ ($author.affiliation$)$endif$
    $else$
    $author$
    $endif$
    $endfor$

Raw content to include in the document’s header may be specified using <span class="nowrap">`header-includes`</span>; however, it is important to mark up this content as raw code for a particular output format, using the [<span class="nowrap">`raw_attribute`</span> extension](#extension-raw_attribute), or it will be interpreted as Markdown. For example:

    header-includes:
    - |
      ```{=latex}
      \let\oldsection\section
      \renewcommand{\section}[1]{\clearpage\oldsection{#1}}
      ```

Note: the <span class="nowrap">`yaml_metadata_block`</span> extension works with <span class="nowrap">`commonmark`</span> as well as <span class="nowrap">`markdown`</span> (and it is enabled by default in <span class="nowrap">`gfm`</span> and <span class="nowrap">`commonmark_x`</span>). However, in these formats the following restrictions apply:

-   The YAML metadata block must occur at the beginning of the document (and there can be only one). If multiple files are given as arguments to pandoc, only the first can be a YAML metadata block.

-   The leaf nodes of the YAML structure are parsed in isolation from each other and from the rest of the document. So, for example, you can’t use a reference link in these contexts if the link definition is somewhere else in the document.

<a href="#backslash-escapes" class="anchor"></a>Backslash escapes <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------------

### <a href="#extension-all_symbols_escapable" class="anchor"></a>Extension: <span class="nowrap">`all_symbols_escapable`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Except inside a code block or inline code, any punctuation or space character preceded by a backslash will be treated literally, even if it would normally indicate formatting. Thus, for example, if one writes

    *\*hello\**

one will get

    <em>*hello*</em>

instead of

    <strong>hello</strong>

This rule is easier to remember than original Markdown’s rule, which allows only the following characters to be backslash-escaped:

    \`*_{}[]()>#+-.!

(However, if the <span class="nowrap">`markdown_strict`</span> format is used, the original Markdown rule will be used.)

A backslash-escaped space is parsed as a nonbreaking space. In TeX output, it will appear as <span class="nowrap">`~`</span>. In HTML and XML output, it will appear as a literal unicode nonbreaking space character (note that it will thus actually look “invisible” in the generated HTML source; you can still use the <a href="#option--ascii%5B" class="option"><span class="nowrap"><code>--ascii</code></span></a> command-line option to make it appear as an explicit entity).

A backslash-escaped newline (i.e. a backslash occurring at the end of a line) is parsed as a hard line break. It will appear in TeX output as <span class="nowrap">`\\`</span> and in HTML as <span class="nowrap">`<br />`</span>. This is a nice alternative to Markdown’s “invisible” way of indicating hard line breaks using two trailing spaces on a line.

Backslash escapes do not work in verbatim contexts.

<a href="#inline-formatting" class="anchor"></a>Inline formatting <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------------

### <a href="#emphasis" class="anchor"></a>Emphasis <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

To *emphasize* some text, surround it with <span class="nowrap">`*`</span>s or <span class="nowrap">`_`</span>, like this:

    This text is _emphasized with underscores_, and this
    is *emphasized with asterisks*.

Double <span class="nowrap">`*`</span> or <span class="nowrap">`_`</span> produces **strong emphasis**:

    This is **strong emphasis** and __with underscores__.

A <span class="nowrap">`*`</span> or <span class="nowrap">`_`</span> character surrounded by spaces, or backslash-escaped, will not trigger emphasis:

    This is * not emphasized *, and \*neither is this\*.

### <a href="#extension-intraword_underscores" class="anchor"></a>Extension: <span class="nowrap">`intraword_underscores`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Because <span class="nowrap">`_`</span> is sometimes used inside words and identifiers, pandoc does not interpret a <span class="nowrap">`_`</span> surrounded by alphanumeric characters as an emphasis marker. If you want to emphasize just part of a word, use <span class="nowrap">`*`</span>:

    feas*ible*, not feas*able*.

### <a href="#strikeout" class="anchor"></a>Strikeout <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

### <a href="#extension-strikeout" class="anchor"></a>Extension: <span class="nowrap">`strikeout`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

To strike out a section of text with a horizontal line, begin and end it with <span class="nowrap">`~~`</span>. Thus, for example,

    This ~~is deleted text.~~

### <a href="#superscripts-and-subscripts" class="anchor"></a>Superscripts and subscripts <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

### <a href="#extension-superscript-subscript" class="anchor"></a>Extension: <span class="nowrap">`superscript`</span>, <span class="nowrap">`subscript`</span> <span class="extension-checkbox" title="enabled by default for:
      + commonmark_x
      + markdown
      + markdown_mmd
      + opml

      disabled by default for:
      - commonmark
      - gfm
      - ipynb
      - markdown_github
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Superscripts may be written by surrounding the superscripted text by <span class="nowrap">`^`</span> characters; subscripts may be written by surrounding the subscripted text by <span class="nowrap">`~`</span> characters. Thus, for example,

    H~2~O is a liquid.  2^10^ is 1024.

The text between <span class="nowrap">`^...^`</span> or <span class="nowrap">`~...~`</span> may not contain spaces or newlines. If the superscripted or subscripted text contains spaces, these spaces must be escaped with backslashes. (This is to prevent accidental superscripting and subscripting through the ordinary use of <span class="nowrap">`~`</span> and <span class="nowrap">`^`</span>, and also bad interactions with footnotes.) Thus, if you want the letter P with ‘a cat’ in subscripts, use <span class="nowrap">`P~a\ cat~`</span>, not <span class="nowrap">`P~a cat~`</span>.

### <a href="#verbatim" class="anchor"></a>Verbatim <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

To make a short span of text verbatim, put it inside backticks:

    What is the difference between `>>=` and `>>`?

If the verbatim text includes a backtick, use double backticks:

    Here is a literal backtick `` ` ``.

(The spaces after the opening backticks and before the closing backticks will be ignored.)

The general rule is that a verbatim span starts with a string of consecutive backticks (optionally followed by a space) and ends with a string of the same number of backticks (optionally preceded by a space).

Note that backslash-escapes (and other Markdown constructs) do not work in verbatim contexts:

    This is a backslash followed by an asterisk: `\*`.

### <a href="#extension-inline_code_attributes" class="anchor"></a>Extension: <span class="nowrap">`inline_code_attributes`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Attributes can be attached to verbatim text, just as with [fenced code blocks](#fenced-code-blocks):

    `<$>`{.haskell}

### <a href="#underline" class="anchor"></a>Underline <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

To underline text, use the <span class="nowrap">`underline`</span> class:

    [Underline]{.underline}

Or, without the <span class="nowrap">`bracketed_spans`</span> extension (but with <span class="nowrap">`native_spans`</span>):

    <span class="underline">Underline</span>

This will work in all output formats that support underline.

### <a href="#small-caps" class="anchor"></a>Small caps <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

To write small caps, use the <span class="nowrap">`smallcaps`</span> class:

    [Small caps]{.smallcaps}

Or, without the <span class="nowrap">`bracketed_spans`</span> extension:

    <span class="smallcaps">Small caps</span>

For compatibility with other Markdown flavors, CSS is also supported:

    <span style="font-variant:small-caps;">Small caps</span>

This will work in all output formats that support small caps.

### <a href="#highlighting" class="anchor"></a>Highlighting <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

To highlight text, use the <span class="nowrap">`mark`</span> class:

    [Mark]{.mark}

Or, without the <span class="nowrap">`bracketed_spans`</span> extension (but with <span class="nowrap">`native_spans`</span>):

    <span class="mark">Mark</span>

This will work in all output formats that support highlighting.

<a href="#math" class="anchor"></a>Math <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------

### <a href="#extension-tex_math_dollars" class="anchor"></a>Extension: <span class="nowrap">`tex_math_dollars`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Anything between two <span class="nowrap">`$`</span> characters will be treated as TeX math. The opening <span class="nowrap">`$`</span> must have a non-space character immediately to its right, while the closing <span class="nowrap">`$`</span> must have a non-space character immediately to its left, and must not be followed immediately by a digit. Thus, <span class="nowrap">`$20,000 and $30,000`</span> won’t parse as math. If for some reason you need to enclose text in literal <span class="nowrap">`$`</span> characters, backslash-escape them and they won’t be treated as math delimiters.

For display math, use <span class="nowrap">`$$`</span> delimiters. (In this case, the delimiters may be separated from the formula by whitespace. However, there can be no blank lines between the opening and closing <span class="nowrap">`$$`</span> delimiters.)

TeX math will be printed in all output formats. How it is rendered depends on the output format:

LaTeX  
It will appear verbatim surrounded by <span class="nowrap">`\(...\)`</span> (for inline math) or <span class="nowrap">`\[...\]`</span> (for display math).

Markdown, Emacs Org mode, ConTeXt, ZimWiki  
It will appear verbatim surrounded by <span class="nowrap">`$...$`</span> (for inline math) or <span class="nowrap">`$$...$$`</span> (for display math).

XWiki  
It will appear verbatim surrounded by <span class="nowrap">`{{formula}}..{{/formula}}`</span>.

reStructuredText  
It will be rendered using an [interpreted text role <span class="nowrap">`:math:`</span>](https://docutils.sourceforge.io/docs/ref/rst/roles.html#math).

AsciiDoc  
For AsciiDoc output math will appear verbatim surrounded by <span class="nowrap">`latexmath:[...]`</span>. For <span class="nowrap">`asciidoc_legacy`</span> the bracketed material will also include inline or display math delimiters.

Texinfo  
It will be rendered inside a <span class="nowrap">`@math`</span> command.

roff man, Jira markup  
It will be rendered verbatim without <span class="nowrap">`$`</span>’s.

MediaWiki, DokuWiki  
It will be rendered inside <span class="nowrap">`<math>`</span> tags.

Textile  
It will be rendered inside <span class="nowrap">`<span class="math">`</span> tags.

RTF, OpenDocument  
It will be rendered, if possible, using Unicode characters, and will otherwise appear verbatim.

ODT  
It will be rendered, if possible, using MathML.

DocBook  
If the <a href="#option--mathml" class="option"><span class="nowrap"><code>--mathml</code></span></a> flag is used, it will be rendered using MathML in an <span class="nowrap">`inlineequation`</span> or <span class="nowrap">`informalequation`</span> tag. Otherwise it will be rendered, if possible, using Unicode characters.

Docx and PowerPoint  
It will be rendered using OMML math markup.

FictionBook2  
If the <a href="#option--webtex" class="option"><span class="nowrap"><code>--webtex</code></span></a> option is used, formulas are rendered as images using CodeCogs or other compatible web service, downloaded and embedded in the e-book. Otherwise, they will appear verbatim.

HTML, Slidy, DZSlides, S5, EPUB  
The way math is rendered in HTML will depend on the command-line options selected. Therefore see [Math rendering in HTML](#math-rendering-in-html) above.

<a href="#raw-html" class="anchor"></a>Raw HTML <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------

### <a href="#extension-raw_html" class="anchor"></a>Extension: <span class="nowrap">`raw_html`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Markdown allows you to insert raw HTML (or DocBook) anywhere in a document (except verbatim contexts, where <span class="nowrap">`<`</span>, <span class="nowrap">`>`</span>, and <span class="nowrap">`&`</span> are interpreted literally). (Technically this is not an extension, since standard Markdown allows it, but it has been made an extension so that it can be disabled if desired.)

The raw HTML is passed through unchanged in HTML, S5, Slidy, Slideous, DZSlides, EPUB, Markdown, CommonMark, Emacs Org mode, and Textile output, and suppressed in other formats.

For a more explicit way of including raw HTML in a Markdown document, see the [<span class="nowrap">`raw_attribute`</span> extension](#extension-raw_attribute).

In the CommonMark format, if <span class="nowrap">`raw_html`</span> is enabled, superscripts, subscripts, strikeouts and small capitals will be represented as HTML. Otherwise, plain-text fallbacks will be used. Note that even if <span class="nowrap">`raw_html`</span> is disabled, tables will be rendered with HTML syntax if they cannot use pipe syntax.

### <a href="#extension-markdown_in_html_blocks" class="anchor"></a>Extension: <span class="nowrap">`markdown_in_html_blocks`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Original Markdown allows you to include HTML “blocks”: blocks of HTML between balanced tags that are separated from the surrounding text with blank lines, and start and end at the left margin. Within these blocks, everything is interpreted as HTML, not Markdown; so (for example), <span class="nowrap">`*`</span> does not signify emphasis.

Pandoc behaves this way when the <span class="nowrap">`markdown_strict`</span> format is used; but by default, pandoc interprets material between HTML block tags as Markdown. Thus, for example, pandoc will turn

    <table>
    <tr>
    <td>*one*</td>
    <td>[a link](https://google.com)</td>
    </tr>
    </table>

into

    <table>
    <tr>
    <td><em>one</em></td>
    <td><a href="https://google.com">a link</a></td>
    </tr>
    </table>

whereas <span class="nowrap">`Markdown.pl`</span> will preserve it as is.

There is one exception to this rule: text between <span class="nowrap">`<script>`</span>, <span class="nowrap">`<style>`</span>, <span class="nowrap">`<pre>`</span>, and <span class="nowrap">`<textarea>`</span> tags is not interpreted as Markdown.

This departure from original Markdown should make it easier to mix Markdown with HTML block elements. For example, one can surround a block of Markdown text with <span class="nowrap">`<div>`</span> tags without preventing it from being interpreted as Markdown.

### <a href="#extension-native_divs" class="anchor"></a>Extension: <span class="nowrap">`native_divs`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Use native pandoc <span class="nowrap">`Div`</span> blocks for content inside <span class="nowrap">`<div>`</span> tags. For the most part this should give the same output as <span class="nowrap">`markdown_in_html_blocks`</span>, but it makes it easier to write pandoc filters to manipulate groups of blocks.

### <a href="#extension-native_spans" class="anchor"></a>Extension: <span class="nowrap">`native_spans`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Use native pandoc <span class="nowrap">`Span`</span> blocks for content inside <span class="nowrap">`<span>`</span> tags. For the most part this should give the same output as <span class="nowrap">`raw_html`</span>, but it makes it easier to write pandoc filters to manipulate groups of inlines.

### <a href="#extension-raw_tex" class="anchor"></a>Extension: <span class="nowrap">`raw_tex`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

In addition to raw HTML, pandoc allows raw LaTeX, TeX, and ConTeXt to be included in a document. Inline TeX commands will be preserved and passed unchanged to the LaTeX and ConTeXt writers. Thus, for example, you can use LaTeX to include BibTeX citations:

    This result was proved in \cite{jones.1967}.

Note that in LaTeX environments, like

    \begin{tabular}{|l|l|}\hline
    Age & Frequency \\ \hline
    18--25  & 15 \\
    26--35  & 33 \\
    36--45  & 22 \\ \hline
    \end{tabular}

the material between the begin and end tags will be interpreted as raw LaTeX, not as Markdown.

For a more explicit and flexible way of including raw TeX in a Markdown document, see the [<span class="nowrap">`raw_attribute`</span> extension](#extension-raw_attribute).

Inline LaTeX is ignored in output formats other than Markdown, LaTeX, Emacs Org mode, and ConTeXt.

### <a href="#generic-raw-attribute" class="anchor"></a>Generic raw attribute <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

### <a href="#extension-raw_attribute" class="anchor"></a>Extension: <span class="nowrap">`raw_attribute`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Inline spans and fenced code blocks with a special kind of attribute will be parsed as raw content with the designated format. For example, the following produces a raw roff <span class="nowrap">`ms`</span> block:

    ```{=ms}
    .MYMACRO
    blah blah
    ```

And the following produces a raw <span class="nowrap">`html`</span> inline element:

    This is `<a>html</a>`{=html}

This can be useful to insert raw xml into <span class="nowrap">`docx`</span> documents, e.g. a pagebreak:

    ```{=openxml}
    <w:p>
      <w:r>
        <w:br w:type="page"/>
      </w:r>
    </w:p>
    ```

The format name should match the target format name (see <a href="#option--to" class="option"><span class="nowrap"><code>-t/--to</code></span></a>, above, for a list, or use <span class="nowrap">`pandoc --list-output-formats`</span>). Use <span class="nowrap">`openxml`</span> for <span class="nowrap">`docx`</span> output, <span class="nowrap">`opendocument`</span> for <span class="nowrap">`odt`</span> output, <span class="nowrap">`html5`</span> for <span class="nowrap">`epub3`</span> output, <span class="nowrap">`html4`</span> for <span class="nowrap">`epub2`</span> output, and <span class="nowrap">`latex`</span>, <span class="nowrap">`beamer`</span>, <span class="nowrap">`ms`</span>, or <span class="nowrap">`html5`</span> for <span class="nowrap">`pdf`</span> output (depending on what you use for <a href="#option--pdf-engine" class="option"><span class="nowrap"><code>--pdf-engine</code></span></a>).

This extension presupposes that the relevant kind of inline code or fenced code block is enabled. Thus, for example, to use a raw attribute with a backtick code block, <span class="nowrap">`backtick_code_blocks`</span> must be enabled.

The raw attribute cannot be combined with regular attributes.

<a href="#latex-macros" class="anchor"></a>LaTeX macros <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------

### <a href="#extension-latex_macros" class="anchor"></a>Extension: <span class="nowrap">`latex_macros`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

When this extension is enabled, pandoc will parse LaTeX macro definitions and apply the resulting macros to all LaTeX math and raw LaTeX. So, for example, the following will work in all output formats, not just LaTeX:

    \newcommand{\tuple}[1]{\langle #1 \rangle}

    $\tuple{a, b, c}$

Note that LaTeX macros will not be applied if they occur inside a raw span or block marked with the [<span class="nowrap">`raw_attribute`</span> extension](#extension-raw_attribute).

When <span class="nowrap">`latex_macros`</span> is disabled, the raw LaTeX and math will not have macros applied. This is usually a better approach when you are targeting LaTeX or PDF.

Macro definitions in LaTeX will be passed through as raw LaTeX only if <span class="nowrap">`latex_macros`</span> is not enabled. Macro definitions in Markdown source (or other formats allowing <span class="nowrap">`raw_tex`</span>) will be passed through regardless of whether <span class="nowrap">`latex_macros`</span> is enabled.

<a href="#links-1" class="anchor"></a>Links <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------

Markdown allows links to be specified in several ways.

### <a href="#automatic-links" class="anchor"></a>Automatic links <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

If you enclose a URL or email address in pointy brackets, it will become a link:

    <https://google.com>
    <[email protected]>

### <a href="#inline-links" class="anchor"></a>Inline links <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

An inline link consists of the link text in square brackets, followed by the URL in parentheses. (Optionally, the URL can be followed by a link title, in quotes.)

    This is an [inline link](/url), and here's [one with
    a title](https://fsf.org "click here for a good time!").

There can be no space between the bracketed part and the parenthesized part. The link text can contain formatting (such as emphasis), but the title cannot.

Email addresses in inline links are not autodetected, so they have to be prefixed with <span class="nowrap">`mailto`</span>:

    [Write me!](mailto:[email protected])

### <a href="#reference-links" class="anchor"></a>Reference links <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

An *explicit* reference link has two parts, the link itself and the link definition, which may occur elsewhere in the document (either before or after the link).

The link consists of link text in square brackets, followed by a label in square brackets. (There cannot be space between the two unless the <span class="nowrap">`spaced_reference_links`</span> extension is enabled.) The link definition consists of the bracketed label, followed by a colon and a space, followed by the URL, and optionally (after a space) a link title either in quotes or in parentheses. The label must not be parseable as a citation (assuming the <span class="nowrap">`citations`</span> extension is enabled): citations take precedence over link labels.

Here are some examples:

    [my label 1]: /foo/bar.html  "My title, optional"
    [my label 2]: /foo
    [my label 3]: https://fsf.org (The Free Software Foundation)
    [my label 4]: /bar#special  'A title in single quotes'

The URL may optionally be surrounded by angle brackets:

    [my label 5]: <http://foo.bar.baz>

The title may go on the next line:

    [my label 3]: https://fsf.org
      "The Free Software Foundation"

Note that link labels are not case sensitive. So, this will work:

    Here is [my link][FOO]

    [Foo]: /bar/baz

In an *implicit* reference link, the second pair of brackets is empty:

    See [my website][].

    [my website]: http://foo.bar.baz

Note: In <span class="nowrap">`Markdown.pl`</span> and most other Markdown implementations, reference link definitions cannot occur in nested constructions such as list items or block quotes. Pandoc lifts this arbitrary-seeming restriction. So the following is fine in pandoc, though not in most other implementations:

    > My block [quote].
    >
    > [quote]: /foo

### <a href="#extension-shortcut_reference_links" class="anchor"></a>Extension: <span class="nowrap">`shortcut_reference_links`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

In a *shortcut* reference link, the second pair of brackets may be omitted entirely:

    See [my website].

    [my website]: http://foo.bar.baz

### <a href="#internal-links" class="anchor"></a>Internal links <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

To link to another section of the same document, use the automatically generated identifier (see [Heading identifiers](#heading-identifiers)). For example:

    See the [Introduction](#introduction).

or

    See the [Introduction].

    [Introduction]: #introduction

Internal links are currently supported for HTML formats (including HTML slide shows and EPUB), LaTeX, and ConTeXt.

<a href="#images" class="anchor"></a>Images <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------

A link immediately preceded by a <span class="nowrap">`!`</span> will be treated as an image. The link text will be used as the image’s alt text:

    ![la lune](lalune.jpg "Voyage to the moon")

    ![movie reel]

    [movie reel]: movie.gif

### <a href="#extension-implicit_figures" class="anchor"></a>Extension: <span class="nowrap">`implicit_figures`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

An image with nonempty alt text, occurring by itself in a paragraph, will be rendered as a figure with a caption. The image’s alt text will be used as the caption.

    ![This is the caption](/url/of/image.png)

How this is rendered depends on the output format. Some output formats (e.g. RTF) do not yet support figures. In those formats, you’ll just get an image in a paragraph by itself, with no caption.

If you just want a regular inline image, just make sure it is not the only thing in the paragraph. One way to do this is to insert a nonbreaking space after the image:

    ![This image won't be a figure](/url/of/image.png)\

Note that in reveal.js slide shows, an image in a paragraph by itself that has the <span class="nowrap">`r-stretch`</span> class will fill the screen, and the caption and figure tags will be omitted.

### <a href="#extension-link_attributes" class="anchor"></a>Extension: <span class="nowrap">`link_attributes`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Attributes can be set on links and images:

    An inline ![image](foo.jpg){#id .class width=30 height=20px}
    and a reference ![image][ref] with attributes.

    [ref]: foo.jpg "optional title" {#id .class key=val key2="val 2"}

(This syntax is compatible with [PHP Markdown Extra](https://michelf.ca/projects/php-markdown/extra/) when only <span class="nowrap">`#id`</span> and <span class="nowrap">`.class`</span> are used.)

For HTML and EPUB, all known HTML5 attributes except <span class="nowrap">`width`</span> and <span class="nowrap">`height`</span> (but including <span class="nowrap">`srcset`</span> and <span class="nowrap">`sizes`</span>) are passed through as is. Unknown attributes are passed through as custom attributes, with <span class="nowrap">`data-`</span> prepended. The other writers ignore attributes that are not specifically supported by their output format.

The <span class="nowrap">`width`</span> and <span class="nowrap">`height`</span> attributes on images are treated specially. When used without a unit, the unit is assumed to be pixels. However, any of the following unit identifiers can be used: <span class="nowrap">`px`</span>, <span class="nowrap">`cm`</span>, <span class="nowrap">`mm`</span>, <span class="nowrap">`in`</span>, <span class="nowrap">`inch`</span> and <span class="nowrap">`%`</span>. There must not be any spaces between the number and the unit. For example:

    ![](file.jpg){ width=50% }

-   Dimensions may be converted to a form that is compatible with the output format (for example, dimensions given in pixels will be converted to inches when converting HTML to LaTeX). Conversion between pixels and physical measurements is affected by the <a href="#option--dpi" class="option"><span class="nowrap"><code>--dpi</code></span></a> option (by default, 96 dpi is assumed, unless the image itself contains dpi information).
-   The <span class="nowrap">`%`</span> unit is generally relative to some available space. For example the above example will render to the following.
    -   HTML: <span class="nowrap">`<img href="file.jpg" style="width: 50%;" />`</span>
    -   LaTeX: <span class="nowrap">`\includegraphics[width=0.5\textwidth,height=\textheight]{file.jpg}`</span> (If you’re using a custom template, you need to configure <span class="nowrap">`graphicx`</span> as in the default template.)
    -   ConTeXt: <span class="nowrap">`\externalfigure[file.jpg][width=0.5\textwidth]`</span>
-   Some output formats have a notion of a class ([ConTeXt](https://wiki.contextgarden.net/Using_Graphics#Multiple_Image_Settings)) or a unique identifier (LaTeX <span class="nowrap">`\caption`</span>), or both (HTML).
-   When no <span class="nowrap">`width`</span> or <span class="nowrap">`height`</span> attributes are specified, the fallback is to look at the image resolution and the dpi metadata embedded in the image file.

<a href="#divs-and-spans" class="anchor"></a>Divs and Spans <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------

Using the <span class="nowrap">`native_divs`</span> and <span class="nowrap">`native_spans`</span> extensions (see [above](#extension-native_divs)), HTML syntax can be used as part of Markdown to create native <span class="nowrap">`Div`</span> and <span class="nowrap">`Span`</span> elements in the pandoc AST (as opposed to raw HTML). However, there is also nicer syntax available:

### <a href="#extension-fenced_divs" class="anchor"></a>Extension: <span class="nowrap">`fenced_divs`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Allow special fenced syntax for native <span class="nowrap">`Div`</span> blocks. A Div starts with a fence containing at least three consecutive colons plus some attributes. The attributes may optionally be followed by another string of consecutive colons.

Note: the <span class="nowrap">`commonmark`</span> parser doesn’t permit colons after the attributes.

The attribute syntax is exactly as in fenced code blocks (see [Extension: <span class="nowrap">`fenced_code_attributes`</span>](#extension-fenced_code_attributes)). As with fenced code blocks, one can use either attributes in curly braces or a single unbraced word, which will be treated as a class name. The Div ends with another line containing a string of at least three consecutive colons. The fenced Div should be separated by blank lines from preceding and following blocks.

Example:

    ::::: {#special .sidebar}
    Here is a paragraph.

    And another.
    :::::

Fenced divs can be nested. Opening fences are distinguished because they *must* have attributes:

    ::: Warning ::::::
    This is a warning.

    ::: Danger
    This is a warning within a warning.
    :::
    ::::::::::::::::::

Fences without attributes are always closing fences. Unlike with fenced code blocks, the number of colons in the closing fence need not match the number in the opening fence. However, it can be helpful for visual clarity to use fences of different lengths to distinguish nested divs from their parents.

### <a href="#extension-bracketed_spans" class="anchor"></a>Extension: <span class="nowrap">`bracketed_spans`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

A bracketed sequence of inlines, as one would use to begin a link, will be treated as a <span class="nowrap">`Span`</span> with attributes if it is followed immediately by attributes:

    [This is *some text*]{.class key="val"}

<a href="#footnotes" class="anchor"></a>Footnotes <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------

### <a href="#extension-footnotes" class="anchor"></a>Extension: <span class="nowrap">`footnotes`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Pandoc’s Markdown allows footnotes, using the following syntax:

    Here is a footnote reference,[^1] and another.[^longnote]

    [^1]: Here is the footnote.

    [^longnote]: Here's one with multiple blocks.

        Subsequent paragraphs are indented to show that they
    belong to the previous footnote.

            { some.code }

        The whole paragraph can be indented, or just the first
        line.  In this way, multi-paragraph footnotes work like
        multi-paragraph list items.

    This paragraph won't be part of the note, because it
    isn't indented.

The identifiers in footnote references may not contain spaces, tabs, newlines, or the characters <span class="nowrap">`^`</span>, <span class="nowrap">`[`</span>, or <span class="nowrap">`]`</span>. These identifiers are used only to correlate the footnote reference with the note itself; in the output, footnotes will be numbered sequentially.

The footnotes themselves need not be placed at the end of the document. They may appear anywhere except inside other block elements (lists, block quotes, tables, etc.). Each footnote should be separated from surrounding content (including other footnotes) by blank lines.

### <a href="#extension-inline_notes" class="anchor"></a>Extension: <span class="nowrap">`inline_notes`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Inline footnotes are also allowed (though, unlike regular notes, they cannot contain multiple paragraphs). The syntax is as follows:

    Here is an inline note.^[Inline notes are easier to write, since
    you don't have to pick an identifier and move down to type the
    note.]

Inline and regular footnotes may be mixed freely.

<a href="#citation-syntax" class="anchor"></a>Citation syntax <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------

### <a href="#extension-citations" class="anchor"></a>Extension: <span class="nowrap">`citations`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

To cite a bibliographic item with an identifier foo, use the syntax <span class="nowrap">`@foo`</span>. Normal citations should be included in square brackets, with semicolons separating distinct items:

    Blah blah [@doe99; @smith2000; @smith2004].

How this is rendered depends on the citation style. In an author-date style, it might render as

    Blah blah (Doe 1999, Smith 2000, 2004).

In a footnote style, it might render as

    Blah blah.[^1]

    [^1]:  John Doe, "Frogs," *Journal of Amphibians* 44 (1999);
    Susan Smith, "Flies," *Journal of Insects* (2000);
    Susan Smith, "Bees," *Journal of Insects* (2004).

See the [CSL user documentation](https://citationstyles.org/authors/) for more information about CSL styles and how they affect rendering.

Unless a citation key starts with a letter, digit, or <span class="nowrap">`_`</span>, and contains only alphanumerics and single internal punctuation characters (<span class="nowrap">`:.#$%&-+?<>~/`</span>), it must be surrounded by curly braces, which are not considered part of the key. In <span class="nowrap">`@Foo_bar.baz.`</span>, the key is <span class="nowrap">`Foo_bar.baz`</span> because the final period is not *internal* punctuation, so it is not included in the key. In <span class="nowrap">`@{Foo_bar.baz.}`</span>, the key is <span class="nowrap">`Foo_bar.baz.`</span>, including the final period. In <span class="nowrap">`@Foo_bar--baz`</span>, the key is <span class="nowrap">`Foo_bar`</span> because the repeated internal punctuation characters terminate the key. The curly braces are recommended if you use URLs as keys: <span class="nowrap">`[@{https://example.com/bib?name=foobar&date=2000}, p.  33]`</span>.

Citation items may optionally include a prefix, a locator, and a suffix. In

    Blah blah [see @doe99, pp. 33-35 and *passim*; @smith04, chap. 1].

the first item (<span class="nowrap">`doe99`</span>) has prefix <span class="nowrap">`see`</span>, locator <span class="nowrap">`pp.  33-35`</span>, and suffix <span class="nowrap">`and *passim*`</span>. The second item (<span class="nowrap">`smith04`</span>) has locator <span class="nowrap">`chap. 1`</span> and no prefix or suffix.

Pandoc uses some heuristics to separate the locator from the rest of the subject. It is sensitive to the locator terms defined in the [CSL locale files](https://github.com/citation-style-language/locales). Either abbreviated or unabbreviated forms are accepted. In the <span class="nowrap">`en-US`</span> locale, locator terms can be written in either singular or plural forms, as <span class="nowrap">`book`</span>, <span class="nowrap">`bk.`</span>/<span class="nowrap">`bks.`</span>; <span class="nowrap">`chapter`</span>, <span class="nowrap">`chap.`</span>/<span class="nowrap">`chaps.`</span>; <span class="nowrap">`column`</span>, <span class="nowrap">`col.`</span>/<span class="nowrap">`cols.`</span>; <span class="nowrap">`figure`</span>, <span class="nowrap">`fig.`</span>/<span class="nowrap">`figs.`</span>; <span class="nowrap">`folio`</span>, <span class="nowrap">`fol.`</span>/<span class="nowrap">`fols.`</span>; <span class="nowrap">`number`</span>, <span class="nowrap">`no.`</span>/<span class="nowrap">`nos.`</span>; <span class="nowrap">`line`</span>, <span class="nowrap">`l.`</span>/<span class="nowrap">`ll.`</span>; <span class="nowrap">`note`</span>, <span class="nowrap">`n.`</span>/<span class="nowrap">`nn.`</span>; <span class="nowrap">`opus`</span>, <span class="nowrap">`op.`</span>/<span class="nowrap">`opp.`</span>; <span class="nowrap">`page`</span>, <span class="nowrap">`p.`</span>/<span class="nowrap">`pp.`</span>; <span class="nowrap">`paragraph`</span>, <span class="nowrap">`para.`</span>/<span class="nowrap">`paras.`</span>; <span class="nowrap">`part`</span>, <span class="nowrap">`pt.`</span>/<span class="nowrap">`pts.`</span>; <span class="nowrap">`section`</span>, <span class="nowrap">`sec.`</span>/<span class="nowrap">`secs.`</span>; <span class="nowrap">`sub verbo`</span>, <span class="nowrap">`s.v.`</span>/<span class="nowrap">`s.vv.`</span>; <span class="nowrap">`verse`</span>, <span class="nowrap">`v.`</span>/<span class="nowrap">`vv.`</span>; <span class="nowrap">`volume`</span>, <span class="nowrap">`vol.`</span>/<span class="nowrap">`vols.`</span>; <span class="nowrap">`¶`</span>/<span class="nowrap">`¶¶`</span>; <span class="nowrap">`§`</span>/<span class="nowrap">`§§`</span>. If no locator term is used, “page” is assumed.

In complex cases, you can force something to be treated as a locator by enclosing it in curly braces or prevent parsing the suffix as locator by prepending curly braces:

    [@smith{ii, A, D-Z}, with a suffix]
    [@smith, {pp. iv, vi-xi, (xv)-(xvii)} with suffix here]
    [@smith{}, 99 years later]

A minus sign (<span class="nowrap">`-`</span>) before the <span class="nowrap">`@`</span> will suppress mention of the author in the citation. This can be useful when the author is already mentioned in the text:

    Smith says blah [-@smith04].

You can also write an author-in-text citation, by omitting the square brackets:

    @smith04 says blah.

    @smith04 [p. 33] says blah.

This will cause the author’s name to be rendered, followed by the bibliographical details. Use this form when you want to make the citation the subject of a sentence.

When you are using a note style, it is usually better to let citeproc create the footnotes from citations rather than writing an explicit note. If you do write an explicit note that contains a citation, note that normal citations will be put in parentheses, while author-in-text citations will not. For this reason, it is sometimes preferable to use the author-in-text style inside notes when using a note style.

<a href="#non-default-extensions" class="anchor"></a>Non-default extensions <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------------------------

The following Markdown syntax extensions are not enabled by default in pandoc, but may be enabled by adding <span class="nowrap">`+EXTENSION`</span> to the format name, where <span class="nowrap">`EXTENSION`</span> is the name of the extension. Thus, for example, <span class="nowrap">`markdown+hard_line_breaks`</span> is Markdown with hard line breaks.

### <a href="#extension-rebase_relative_paths" class="anchor"></a>Extension: <span class="nowrap">`rebase_relative_paths`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Rewrite relative paths for Markdown links and images, depending on the path of the file containing the link or image link. For each link or image, pandoc will compute the directory of the containing file, relative to the working directory, and prepend the resulting path to the link or image path.

The use of this extension is best understood by example. Suppose you have a subdirectory for each chapter of a book, <span class="nowrap">`chap1`</span>, <span class="nowrap">`chap2`</span>, <span class="nowrap">`chap3`</span>. Each contains a file <span class="nowrap">`text.md`</span> and a number of images used in the chapter. You would like to have <span class="nowrap">`![image](spider.jpg)`</span> in <span class="nowrap">`chap1/text.md`</span> refer to <span class="nowrap">`chap1/spider.jpg`</span> and <span class="nowrap">`![image](spider.jpg)`</span> in <span class="nowrap">`chap2/text.md`</span> refer to <span class="nowrap">`chap2/spider.jpg`</span>. To do this, use

    pandoc chap*/*.md -f markdown+rebase_relative_paths

Without this extension, you would have to use <span class="nowrap">`![image](chap1/spider.jpg)`</span> in <span class="nowrap">`chap1/text.md`</span> and <span class="nowrap">`![image](chap2/spider.jpg)`</span> in <span class="nowrap">`chap2/text.md`</span>. Links with relative paths will be rewritten in the same way as images.

Absolute paths and URLs are not changed. Neither are empty paths or paths consisting entirely of a fragment, e.g., <span class="nowrap">`#foo`</span>.

Note that relative paths in reference links and images will be rewritten relative to the file containing the link reference definition, not the file containing the reference link or image itself, if these differ.

### <a href="#extension-mark" class="anchor"></a>Extension: <span class="nowrap">`mark`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

To highlight out a section of text, begin and end it with with <span class="nowrap">`==`</span>. Thus, for example,

    This ==is deleted text.==

### <a href="#extension-attributes" class="anchor"></a>Extension: <span class="nowrap">`attributes`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Allows attributes to be attached to any inline or block-level element when parsing <span class="nowrap">`commonmark`</span>. The syntax for the attributes is the same as that used in [<span class="nowrap">`header_attributes`</span>](#extension-header_attributes).

-   Attributes that occur immediately after an inline element affect that element. If they follow a space, then they belong to the space. (Hence, this option subsumes <span class="nowrap">`inline_code_attributes`</span> and <span class="nowrap">`link_attributes`</span>.)
-   Attributes that occur immediately before a block element, on a line by themselves, affect that element.
-   Consecutive attribute specifiers may be used, either for blocks or for inlines. Their attributes will be combined.
-   Attributes that occur at the end of the text of a Setext or ATX heading (separated by whitespace from the text) affect the heading element. (Hence, this option subsumes <span class="nowrap">`header_attributes`</span>.)
-   Attributes that occur after the opening fence in a fenced code block affect the code block element. (Hence, this option subsumes <span class="nowrap">`fenced_code_attributes`</span>.)
-   Attributes that occur at the end of a reference link definition affect links that refer to that definition.

Note that pandoc’s AST does not currently allow attributes to be attached to arbitrary elements. Hence a Span or Div container will be added if needed.

### <a href="#extension-old_dashes" class="anchor"></a>Extension: <span class="nowrap">`old_dashes`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Selects the pandoc &lt;= 1.8.2.1 behavior for parsing smart dashes: <span class="nowrap">`-`</span> before a numeral is an en-dash, and <span class="nowrap">`--`</span> is an em-dash. This option only has an effect if <span class="nowrap">`smart`</span> is enabled. It is selected automatically for <span class="nowrap">`textile`</span> input.

### <a href="#extension-angle_brackets_escapable" class="anchor"></a>Extension: <span class="nowrap">`angle_brackets_escapable`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Allow <span class="nowrap">`<`</span> and <span class="nowrap">`>`</span> to be backslash-escaped, as they can be in GitHub flavored Markdown but not original Markdown. This is implied by pandoc’s default <span class="nowrap">`all_symbols_escapable`</span>.

### <a href="#extension-lists_without_preceding_blankline" class="anchor"></a>Extension: <span class="nowrap">`lists_without_preceding_blankline`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Allow a list to occur right after a paragraph, with no intervening blank space.

### <a href="#extension-four_space_rule" class="anchor"></a>Extension: <span class="nowrap">`four_space_rule`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Selects the pandoc &lt;= 2.0 behavior for parsing lists, so that four spaces indent are needed for list item continuation paragraphs.

### <a href="#extension-spaced_reference_links" class="anchor"></a>Extension: <span class="nowrap">`spaced_reference_links`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Allow whitespace between the two components of a reference link, for example,

    [foo] [bar].

### <a href="#extension-hard_line_breaks" class="anchor"></a>Extension: <span class="nowrap">`hard_line_breaks`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Causes all newlines within a paragraph to be interpreted as hard line breaks instead of spaces.

### <a href="#extension-ignore_line_breaks" class="anchor"></a>Extension: <span class="nowrap">`ignore_line_breaks`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Causes newlines within a paragraph to be ignored, rather than being treated as spaces or as hard line breaks. This option is intended for use with East Asian languages where spaces are not used between words, but text is divided into lines for readability.

### <a href="#extension-east_asian_line_breaks" class="anchor"></a>Extension: <span class="nowrap">`east_asian_line_breaks`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Causes newlines within a paragraph to be ignored, rather than being treated as spaces or as hard line breaks, when they occur between two East Asian wide characters. This is a better choice than <span class="nowrap">`ignore_line_breaks`</span> for texts that include a mix of East Asian wide characters and other characters.

### <a href="#extension-emoji" class="anchor"></a>Extension: <span class="nowrap">`emoji`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Parses textual emojis like <span class="nowrap">`:smile:`</span> as Unicode emoticons.

### <a href="#extension-tex_math_gfm" class="anchor"></a>Extension: <span class="nowrap">`tex_math_gfm`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Supports two GitHub-specific formats for math. Inline math: <span class="nowrap">`` $`e=mc^2`$ ``</span>.

Display math:

    ``` math
    e=mc^2
    ```

### <a href="#extension-tex_math_single_backslash" class="anchor"></a>Extension: <span class="nowrap">`tex_math_single_backslash`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Causes anything between <span class="nowrap">`\(`</span> and <span class="nowrap">`\)`</span> to be interpreted as inline TeX math, and anything between <span class="nowrap">`\[`</span> and <span class="nowrap">`\]`</span> to be interpreted as display TeX math. Note: a drawback of this extension is that it precludes escaping <span class="nowrap">`(`</span> and <span class="nowrap">`[`</span>.

### <a href="#extension-tex_math_double_backslash" class="anchor"></a>Extension: <span class="nowrap">`tex_math_double_backslash`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Causes anything between <span class="nowrap">`\\(`</span> and <span class="nowrap">`\\)`</span> to be interpreted as inline TeX math, and anything between <span class="nowrap">`\\[`</span> and <span class="nowrap">`\\]`</span> to be interpreted as display TeX math.

### <a href="#extension-markdown_attribute" class="anchor"></a>Extension: <span class="nowrap">`markdown_attribute`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

By default, pandoc interprets material inside block-level tags as Markdown. This extension changes the behavior so that Markdown is only parsed inside block-level tags if the tags have the attribute <span class="nowrap">`markdown=1`</span>.

### <a href="#extension-mmd_title_block" class="anchor"></a>Extension: <span class="nowrap">`mmd_title_block`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Enables a [MultiMarkdown](https://fletcherpenney.net/multimarkdown/) style title block at the top of the document, for example:

    Title:   My title
    Author:  John Doe
    Date:    September 1, 2008
    Comment: This is a sample mmd title block, with
             a field spanning multiple lines.

See the MultiMarkdown documentation for details. If <span class="nowrap">`pandoc_title_block`</span> or <span class="nowrap">`yaml_metadata_block`</span> is enabled, it will take precedence over <span class="nowrap">`mmd_title_block`</span>.

### <a href="#extension-abbreviations" class="anchor"></a>Extension: <span class="nowrap">`abbreviations`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Parses PHP Markdown Extra abbreviation keys, like

    *[HTML]: Hypertext Markup Language

Note that the pandoc document model does not support abbreviations, so if this extension is enabled, abbreviation keys are simply skipped (as opposed to being parsed as paragraphs).

### <a href="#extension-alerts" class="anchor"></a>Extension: <span class="nowrap">`alerts`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Supports [GitHub-style Markdown alerts](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#alerts), like

    > [!TIP]
    > Helpful advice for doing things better or more easily.

### <a href="#extension-autolink_bare_uris" class="anchor"></a>Extension: <span class="nowrap">`autolink_bare_uris`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Makes all absolute URIs into links, even when not surrounded by pointy braces <span class="nowrap">`<...>`</span>.

### <a href="#extension-mmd_link_attributes" class="anchor"></a>Extension: <span class="nowrap">`mmd_link_attributes`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Parses MultiMarkdown-style key-value attributes on link and image references. This extension should not be confused with the [<span class="nowrap">`link_attributes`</span>](#extension-link_attributes) extension.

    This is a reference ![image][ref] with MultiMarkdown attributes.

    [ref]: https://path.to/image "Image title" width=20px height=30px
           id=myId class="myClass1 myClass2"

### <a href="#extension-mmd_header_identifiers" class="anchor"></a>Extension: <span class="nowrap">`mmd_header_identifiers`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Parses MultiMarkdown-style heading identifiers (in square brackets, after the heading but before any trailing <span class="nowrap">`#`</span>s in an ATX heading).

### <a href="#extension-compact_definition_lists" class="anchor"></a>Extension: <span class="nowrap">`compact_definition_lists`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Activates the definition list syntax of pandoc 1.12.x and earlier. This syntax differs from the one described above under [Definition lists](#definition-lists) in several respects:

-   No blank line is required between consecutive items of the definition list.
-   To get a “tight” or “compact” list, omit space between consecutive items; the space between a term and its definition does not affect anything.
-   Lazy wrapping of paragraphs is not allowed: the entire definition must be indented four spaces.<a href="#fn4" id="fnref4" class="footnote-ref"><sup>4</sup></a>

### <a href="#extension-gutenberg" class="anchor"></a>Extension: <span class="nowrap">`gutenberg`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Use [Project Gutenberg](https://www.gutenberg.org) conventions for <span class="nowrap">`plain`</span> output: all-caps for strong emphasis, surround by underscores for regular emphasis, add extra blank space around headings.

### <a href="#extension-sourcepos" class="anchor"></a>Extension: <span class="nowrap">`sourcepos`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Include source position attributes when parsing <span class="nowrap">`commonmark`</span>. For elements that accept attributes, a <span class="nowrap">`data-pos`</span> attribute is added; other elements are placed in a surrounding Div or Span element with a <span class="nowrap">`data-pos`</span> attribute.

### <a href="#extension-short_subsuperscripts" class="anchor"></a>Extension: <span class="nowrap">`short_subsuperscripts`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Parse MultiMarkdown-style subscripts and superscripts, which start with a ‘~’ or ‘^’ character, respectively, and include the alphanumeric sequence that follows. For example:

    x^2 = 4

or

    Oxygen is O~2.

### <a href="#extension-wikilinks_title_after_pipe" class="anchor"></a>Extension: <span class="nowrap">`wikilinks_title_after_pipe`</span> <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

Pandoc supports multiple Markdown wikilink syntaxes, regardless of whether the title is before or after the pipe.

Using <a href="#option--from" class="option"><span class="nowrap"><code>--from=markdown+wikilinks_title_after_pipe</code></span></a> results in

    [[URL|title]]

while using <a href="#option--from" class="option"><span class="nowrap"><code>--from=markdown+wikilinks_title_before_pipe</code></span></a> results in

    [[title|URL]]

<a href="#markdown-variants" class="anchor"></a>Markdown variants <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------------

In addition to pandoc’s extended Markdown, the following Markdown variants are supported:

-   <span class="nowrap">`markdown_phpextra`</span> (PHP Markdown Extra)
-   <span class="nowrap">`markdown_github`</span> (deprecated GitHub-Flavored Markdown)
-   <span class="nowrap">`markdown_mmd`</span> (MultiMarkdown)
-   <span class="nowrap">`markdown_strict`</span> (Markdown.pl)
-   <span class="nowrap">`commonmark`</span> (CommonMark)
-   <span class="nowrap">`gfm`</span> (Github-Flavored Markdown)
-   <span class="nowrap">`commonmark_x`</span> (CommonMark with many pandoc extensions)

To see which extensions are supported for a given format, and which are enabled by default, you can use the command

    pandoc --list-extensions=FORMAT

where <span class="nowrap">`FORMAT`</span> is replaced with the name of the format.

Note that the list of extensions for <span class="nowrap">`commonmark`</span>, <span class="nowrap">`gfm`</span>, and <span class="nowrap">`commonmark_x`</span> are defined relative to default commonmark. So, for example, <span class="nowrap">`backtick_code_blocks`</span> does not appear as an extension, since it is enabled by default and cannot be disabled.

<a href="#citations" class="anchor"></a>Citations <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=================================================================================================================

When the <a href="#option--citeproc" class="option"><span class="nowrap"><code>--citeproc</code></span></a> option is used, pandoc can automatically generate citations and a bibliography in a number of styles. Basic usage is

    pandoc --citeproc myinput.txt

To use this feature, you will need to have

-   a document containing citations (see [Citation syntax](#citation-syntax));
-   a source of bibliographic data: either an external bibliography file or a list of <span class="nowrap">`references`</span> in the document’s YAML metadata;
-   optionally, a [CSL](https://docs.citationstyles.org/en/stable/specification.html) citation style.

<a href="#specifying-bibliographic-data" class="anchor"></a>Specifying bibliographic data <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------------------------------------

You can specify an external bibliography using the <span class="nowrap">`bibliography`</span> metadata field in a YAML metadata section or the <a href="#option--bibliography" class="option"><span class="nowrap"><code>--bibliography</code></span></a> command line argument. If you want to use multiple bibliography files, you can supply multiple <a href="#option--bibliography" class="option"><span class="nowrap"><code>--bibliography</code></span></a> arguments or set <span class="nowrap">`bibliography`</span> metadata field to YAML array. A bibliography may have any of these formats:

<table><thead><tr class="header"><th style="text-align: left;">Format</th><th>File extension</th></tr></thead><tbody><tr class="odd"><td style="text-align: left;">BibLaTeX</td><td>.bib</td></tr><tr class="even"><td style="text-align: left;">BibTeX</td><td>.bibtex</td></tr><tr class="odd"><td style="text-align: left;">CSL JSON</td><td>.json</td></tr><tr class="even"><td style="text-align: left;">CSL YAML</td><td>.yaml</td></tr><tr class="odd"><td style="text-align: left;">RIS</td><td>.ris</td></tr></tbody></table>

Note that <span class="nowrap">`.bib`</span> can be used with both BibTeX and BibLaTeX files; use the extension <span class="nowrap">`.bibtex`</span> to force interpretation as BibTeX.

In BibTeX and BibLaTeX databases, pandoc parses LaTeX markup inside fields such as <span class="nowrap">`title`</span>; in CSL YAML databases, pandoc Markdown; and in CSL JSON databases, an [HTML-like markup](https://citeproc-js.readthedocs.io/en/latest/csl-json/markup.html#html-like-formatting-tags):

<span class="nowrap">`<i>...</i>`</span>  
italics

<span class="nowrap">`<b>...</b>`</span>  
bold

<span class="nowrap">`<span style="font-variant:small-caps;">...</span>`</span> or <span class="nowrap">`<sc>...</sc>`</span>  
small capitals

<span class="nowrap">`<sub>...</sub>`</span>  
subscript

<span class="nowrap">`<sup>...</sup>`</span>  
superscript

<span class="nowrap">`<span class="nocase">...</span>`</span>  
prevent a phrase from being capitalized as title case

As an alternative to specifying a bibliography file using <a href="#option--bibliography" class="option"><span class="nowrap"><code>--bibliography</code></span></a> or the YAML metadata field <span class="nowrap">`bibliography`</span>, you can include the citation data directly in the <span class="nowrap">`references`</span> field of the document’s YAML metadata. The field should contain an array of YAML-encoded references, for example:

    ---
    references:
    - type: article-journal
      id: WatsonCrick1953
      author:
      - family: Watson
        given: J. D.
      - family: Crick
        given: F. H. C.
      issued:
        date-parts:
        - - 1953
          - 4
          - 25
      title: 'Molecular structure of nucleic acids: a structure for
        deoxyribose nucleic acid'
      title-short: Molecular structure of nucleic acids
      container-title: Nature
      volume: 171
      issue: 4356
      page: 737-738
      DOI: 10.1038/171737a0
      URL: https://www.nature.com/articles/171737a0
      language: en-GB
    ...

If both an external bibliography and inline (YAML metadata) references are provided, both will be used. In case of conflicting <span class="nowrap">`id`</span>s, the inline references will take precedence.

Note that pandoc can be used to produce such a YAML metadata section from a BibTeX, BibLaTeX, or CSL JSON bibliography:

    pandoc chem.bib -s -f biblatex -t markdown
    pandoc chem.json -s -f csljson -t markdown

Indeed, pandoc can convert between any of these citation formats:

    pandoc chem.bib -s -f biblatex -t csljson
    pandoc chem.yaml -s -f markdown -t biblatex

Running pandoc on a bibliography file with the <a href="#option--citeproc" class="option"><span class="nowrap"><code>--citeproc</code></span></a> option will create a formatted bibliography in the format of your choice:

    pandoc chem.bib -s --citeproc -o chem.html
    pandoc chem.bib -s --citeproc -o chem.pdf

### <a href="#capitalization-in-titles" class="anchor"></a>Capitalization in titles <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

If you are using a bibtex or biblatex bibliography, then observe the following rules:

-   English titles should be in title case. Non-English titles should be in sentence case, and the <span class="nowrap">`langid`</span> field in biblatex should be set to the relevant language. (The following values are treated as English: <span class="nowrap">`american`</span>, <span class="nowrap">`british`</span>, <span class="nowrap">`canadian`</span>, <span class="nowrap">`english`</span>, <span class="nowrap">`australian`</span>, <span class="nowrap">`newzealand`</span>, <span class="nowrap">`USenglish`</span>, or <span class="nowrap">`UKenglish`</span>.)

-   As is standard with bibtex/biblatex, proper names should be protected with curly braces so that they won’t be lowercased in styles that call for sentence case. For example:

        title = {My Dinner with {Andre}}

-   In addition, words that should remain lowercase (or camelCase) should be protected:

        title = {Spin Wave Dispersion on the {nm} Scale}

    Though this is not necessary in bibtex/biblatex, it is necessary with citeproc, which stores titles internally in sentence case, and converts to title case in styles that require it. Here we protect “nm” so that it doesn’t get converted to “Nm” at this stage.

If you are using a CSL bibliography (either JSON or YAML), then observe the following rules:

-   All titles should be in sentence case.

-   Use the <span class="nowrap">`language`</span> field for non-English titles to prevent their conversion to title case in styles that call for this. (Conversion happens only if <span class="nowrap">`language`</span> begins with <span class="nowrap">`en`</span> or is left empty.)

-   Protect words that should not be converted to title case using this syntax:

        Spin wave dispersion on the <span class="nocase">nm</span> scale

### <a href="#conference-papers-published-vs.-unpublished" class="anchor"></a>Conference Papers, Published vs. Unpublished <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

For a formally published conference paper, use the biblatex entry type <span class="nowrap">`inproceedings`</span> (which will be mapped to CSL <span class="nowrap">`paper-conference`</span>).

For an unpublished manuscript, use the biblatex entry type <span class="nowrap">`unpublished`</span> without an <span class="nowrap">`eventtitle`</span> field (this entry type will be mapped to CSL <span class="nowrap">`manuscript`</span>).

For a talk, an unpublished conference paper, or a poster presentation, use the biblatex entry type <span class="nowrap">`unpublished`</span> with an <span class="nowrap">`eventtitle`</span> field (this entry type will be mapped to CSL <span class="nowrap">`speech`</span>). Use the biblatex <span class="nowrap">`type`</span> field to indicate the type, e.g. “Paper”, or “Poster”. <span class="nowrap">`venue`</span> and <span class="nowrap">`eventdate`</span> may be useful too, though <span class="nowrap">`eventdate`</span> will not be rendered by most CSL styles. Note that <span class="nowrap">`venue`</span> is for the event’s venue, unlike <span class="nowrap">`location`</span> which describes the publisher’s location; do not use the latter for an unpublished conference paper.

<a href="#specifying-a-citation-style" class="anchor"></a>Specifying a citation style <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------------------------------

Citations and references can be formatted using any style supported by the [Citation Style Language](https://citationstyles.org), listed in the [Zotero Style Repository](https://www.zotero.org/styles). These files are specified using the <a href="#option--csl" class="option"><span class="nowrap"><code>--csl</code></span></a> option or the <span class="nowrap">`csl`</span> (or <span class="nowrap">`citation-style`</span>) metadata field. By default, pandoc will use the [Chicago Manual of Style](https://chicagomanualofstyle.org) author-date format. (You can override this default by copying a CSL style of your choice to <span class="nowrap">`default.csl`</span> in your user data directory.) The CSL project provides further information on [finding and editing styles](https://citationstyles.org/authors/).

The <a href="#option--citation-abbreviations" class="option"><span class="nowrap"><code>--citation-abbreviations</code></span></a> option (or the <span class="nowrap">`citation-abbreviations`</span> metadata field) may be used to specify a JSON file containing abbreviations of journals that should be used in formatted bibliographies when <span class="nowrap">`form="short"`</span> is specified. The format of the file can be illustrated with an example:

    { "default": {
        "container-title": {
                "Lloyd's Law Reports": "Lloyd's Rep",
                "Estates Gazette": "EG",
                "Scots Law Times": "SLT"
        }
      }
    }

<a href="#citations-in-note-styles" class="anchor"></a>Citations in note styles <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------------------------

Pandoc’s citation processing is designed to allow you to move between author-date, numerical, and note styles without modifying the Markdown source. When you’re using a note style, avoid inserting footnotes manually. Instead, insert citations just as you would in an author-date style—for example,

    Blah blah [@foo, p. 33].

The footnote will be created automatically. Pandoc will take care of removing the space and moving the note before or after the period, depending on the setting of <span class="nowrap">`notes-after-punctuation`</span>, as described below in [Other relevant metadata fields](#other-relevant-metadata-fields).

In some cases you may need to put a citation inside a regular footnote. Normal citations in footnotes (such as <span class="nowrap">`[@foo, p. 33]`</span>) will be rendered in parentheses. In-text citations (such as <span class="nowrap">`@foo [p. 33]`</span>) will be rendered without parentheses. (A comma will be added if appropriate.) Thus:

    [^1]:  Some studies [@foo; @bar, p. 33] show that
    frubulicious zoosnaps are quantical.  For a survey
    of the literature, see @baz [chap. 1].

<a href="#placement-of-the-bibliography" class="anchor"></a>Placement of the bibliography <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------------------------------------

If the style calls for a list of works cited, it will be placed in a div with id <span class="nowrap">`refs`</span>, if one exists:

    ::: {#refs}
    :::

Otherwise, it will be placed at the end of the document. Generation of the bibliography can be suppressed by setting <span class="nowrap">`suppress-bibliography: true`</span> in the YAML metadata.

If you wish the bibliography to have a section heading, you can set <span class="nowrap">`reference-section-title`</span> in the metadata, or put the heading at the beginning of the div with id <span class="nowrap">`refs`</span> (if you are using it) or at the end of your document:

    last paragraph...

    # References

The bibliography will be inserted after this heading. Note that the <span class="nowrap">`unnumbered`</span> class will be added to this heading, so that the section will not be numbered.

If you want to put the bibliography into a variable in your template, one way to do that is to put the div with id <span class="nowrap">`refs`</span> into a metadata field, e.g.

    ---
    refs: |
       ::: {#refs}
       :::
    ...

You can then put the variable <span class="nowrap">`$refs$`</span> into your template where you want the bibliography to be placed.

<a href="#including-uncited-items-in-the-bibliography" class="anchor"></a>Including uncited items in the bibliography <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

If you want to include items in the bibliography without actually citing them in the body text, you can define a dummy <span class="nowrap">`nocite`</span> metadata field and put the citations there:

    ---
    nocite: |
      @item1, @item2
    ...

    @item3

In this example, the document will contain a citation for <span class="nowrap">`item3`</span> only, but the bibliography will contain entries for <span class="nowrap">`item1`</span>, <span class="nowrap">`item2`</span>, and <span class="nowrap">`item3`</span>.

It is possible to create a bibliography with all the citations, whether or not they appear in the document, by using a wildcard:

    ---
    nocite: |
      @*
    ...

For LaTeX output, you can also use [<span class="nowrap">`natbib`</span>](https://ctan.org/pkg/natbib) or [<span class="nowrap">`biblatex`</span>](https://ctan.org/pkg/biblatex) to render the bibliography. In order to do so, specify bibliography files as outlined above, and add <a href="#option--natbib" class="option"><span class="nowrap"><code>--natbib</code></span></a> or <a href="#option--biblatex" class="option"><span class="nowrap"><code>--biblatex</code></span></a> argument to pandoc invocation. Bear in mind that bibliography files have to be in either BibTeX (for <a href="#option--natbib" class="option"><span class="nowrap"><code>--natbib</code></span></a>) or BibLaTeX (for <a href="#option--biblatex" class="option"><span class="nowrap"><code>--biblatex</code></span></a>) format.

<a href="#other-relevant-metadata-fields" class="anchor"></a>Other relevant metadata fields <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------------------------------------

A few other metadata fields affect bibliography formatting:

<span class="nowrap">`link-citations`</span>  
If true, citations will be hyperlinked to the corresponding bibliography entries (for author-date and numerical styles only). Defaults to false.

<span class="nowrap">`link-bibliography`</span>  
If true, DOIs, PMCIDs, PMID, and URLs in bibliographies will be rendered as hyperlinks. (If an entry contains a DOI, PMCID, PMID, or URL, but none of these fields are rendered by the style, then the title, or in the absence of a title the whole entry, will be hyperlinked.) Defaults to true.

<span class="nowrap">`lang`</span>  
The <span class="nowrap">`lang`</span> field will affect how the style is localized, for example in the translation of labels, the use of quotation marks, and the way items are sorted. (For backwards compatibility, <span class="nowrap">`locale`</span> may be used instead of <span class="nowrap">`lang`</span>, but this use is deprecated.)

A BCP 47 language tag is expected: for example, <span class="nowrap">`en`</span>, <span class="nowrap">`de`</span>, <span class="nowrap">`en-US`</span>, <span class="nowrap">`fr-CA`</span>, <span class="nowrap">`ug-Cyrl`</span>. The unicode extension syntax (after <span class="nowrap">`-u-`</span>) may be used to specify options for collation (sorting) more precisely. Here are some examples:

-   <span class="nowrap">`zh-u-co-pinyin`</span>: Chinese with the Pinyin collation.
-   <span class="nowrap">`es-u-co-trad`</span>: Spanish with the traditional collation (with <span class="nowrap">`Ch`</span> sorting after <span class="nowrap">`C`</span>).
-   <span class="nowrap">`fr-u-kb`</span>: French with “backwards” accent sorting (with <span class="nowrap">`coté`</span> sorting after <span class="nowrap">`côte`</span>).
-   <span class="nowrap">`en-US-u-kf-upper`</span>: English with uppercase letters sorting before lower (default is lower before upper).

<span class="nowrap">`notes-after-punctuation`</span>  
If true (the default for note styles), pandoc will put footnote references or superscripted numerical citations after following punctuation. For example, if the source contains <span class="nowrap">`blah blah [@jones99].`</span>, the result will look like <span class="nowrap">`blah blah.[^1]`</span>, with the note moved after the period and the space collapsed. If false, the space will still be collapsed, but the footnote will not be moved after the punctuation. The option may also be used in numerical styles that use superscripts for citation numbers (but for these styles the default is not to move the citation).

<a href="#slide-shows" class="anchor"></a>Slide shows <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=====================================================================================================================

You can use pandoc to produce an HTML + JavaScript slide presentation that can be viewed via a web browser. There are five ways to do this, using [S5](https://meyerweb.com/eric/tools/s5/), [DZSlides](https://paulrouget.com/dzslides/), [Slidy](https://www.w3.org/Talks/Tools/Slidy2/), [Slideous](https://goessner.net/articles/slideous/), or [reveal.js](https://revealjs.com/). You can also produce a PDF slide show using LaTeX [<span class="nowrap">`beamer`</span>](https://ctan.org/pkg/beamer), or slide shows in Microsoft [PowerPoint](https://en.wikipedia.org/wiki/Microsoft_PowerPoint) format.

Here’s the Markdown source for a simple slide show, <span class="nowrap">`habits.txt`</span>:

    % Habits
    % John Doe
    % March 22, 2005

    # In the morning

    ## Getting up

    - Turn off alarm
    - Get out of bed

    ## Breakfast

    - Eat eggs
    - Drink coffee

    # In the evening

    ## Dinner

    - Eat spaghetti
    - Drink wine

    ------------------

    ![picture of spaghetti](images/spaghetti.jpg)

    ## Going to sleep

    - Get in bed
    - Count sheep

To produce an HTML/JavaScript slide show, simply type

    pandoc -t FORMAT -s habits.txt -o habits.html

where <span class="nowrap">`FORMAT`</span> is either <span class="nowrap">`s5`</span>, <span class="nowrap">`slidy`</span>, <span class="nowrap">`slideous`</span>, <span class="nowrap">`dzslides`</span>, or <span class="nowrap">`revealjs`</span>.

For Slidy, Slideous, reveal.js, and S5, the file produced by pandoc with the <a href="#option--standalone" class="option"><span class="nowrap"><code>-s/--standalone</code></span></a> option embeds a link to JavaScript and CSS files, which are assumed to be available at the relative path <span class="nowrap">`s5/default`</span> (for S5), <span class="nowrap">`slideous`</span> (for Slideous), <span class="nowrap">`reveal.js`</span> (for reveal.js), or at the Slidy website at <span class="nowrap">`w3.org`</span> (for Slidy). (These paths can be changed by setting the <span class="nowrap">`slidy-url`</span>, <span class="nowrap">`slideous-url`</span>, <span class="nowrap">`revealjs-url`</span>, or <span class="nowrap">`s5-url`</span> variables; see [Variables for HTML slides](#variables-for-html-slides), above.) For DZSlides, the (relatively short) JavaScript and CSS are included in the file by default.

With all HTML slide formats, the <a href="#option--self-contained%5B" class="option"><span class="nowrap"><code>--self-contained</code></span></a> option can be used to produce a single file that contains all of the data necessary to display the slide show, including linked scripts, stylesheets, images, and videos.

To produce a PDF slide show using beamer, type

    pandoc -t beamer habits.txt -o habits.pdf

Note that a reveal.js slide show can also be converted to a PDF by printing it to a file from the browser.

To produce a PowerPoint slide show, type

    pandoc habits.txt -o habits.pptx

<a href="#structuring-the-slide-show" class="anchor"></a>Structuring the slide show <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------------------------------

By default, the *slide level* is the highest heading level in the hierarchy that is followed immediately by content, and not another heading, somewhere in the document. In the example above, level-1 headings are always followed by level-2 headings, which are followed by content, so the slide level is 2. This default can be overridden using the <a href="#option--slide-level" class="option"><span class="nowrap"><code>--slide-level</code></span></a> option.

The document is carved up into slides according to the following rules:

-   A horizontal rule always starts a new slide.

-   A heading at the slide level always starts a new slide.

-   Headings *below* the slide level in the hierarchy create headings *within* a slide. (In beamer, a “block” will be created. If the heading has the class <span class="nowrap">`example`</span>, an <span class="nowrap">`exampleblock`</span> environment will be used; if it has the class <span class="nowrap">`alert`</span>, an <span class="nowrap">`alertblock`</span> will be used; otherwise a regular <span class="nowrap">`block`</span> will be used.)

-   Headings *above* the slide level in the hierarchy create “title slides,” which just contain the section title and help to break the slide show into sections. Non-slide content under these headings will be included on the title slide (for HTML slide shows) or in a subsequent slide with the same title (for beamer).

-   A title page is constructed automatically from the document’s title block, if present. (In the case of beamer, this can be disabled by commenting out some lines in the default template.)

These rules are designed to support many different styles of slide show. If you don’t care about structuring your slides into sections and subsections, you can either just use level-1 headings for all slides (in that case, level 1 will be the slide level) or you can set <a href="#option--slide-level" class="option"><span class="nowrap"><code>--slide-level=0</code></span></a>.

Note: in reveal.js slide shows, if slide level is 2, a two-dimensional layout will be produced, with level-1 headings building horizontally and level-2 headings building vertically. It is not recommended that you use deeper nesting of section levels with reveal.js unless you set <a href="#option--slide-level" class="option"><span class="nowrap"><code>--slide-level=0</code></span></a> (which lets reveal.js produce a one-dimensional layout and only interprets horizontal rules as slide boundaries).

### <a href="#powerpoint-layout-choice" class="anchor"></a>PowerPoint layout choice <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

When creating slides, the pptx writer chooses from a number of pre-defined layouts, based on the content of the slide:

Title Slide  
This layout is used for the initial slide, which is generated and filled from the metadata fields <span class="nowrap">`date`</span>, <span class="nowrap">`author`</span>, and <span class="nowrap">`title`</span>, if they are present.

Section Header  
This layout is used for what pandoc calls “title slides”, i.e. slides which start with a header which is above the slide level in the hierarchy.

Two Content  
This layout is used for two-column slides, i.e. slides containing a div with class <span class="nowrap">`columns`</span> which contains at least two divs with class <span class="nowrap">`column`</span>.

Comparison  
This layout is used instead of “Two Content” for any two-column slides in which at least one column contains text followed by non-text (e.g. an image or a table).

Content with Caption  
This layout is used for any non-two-column slides which contain text followed by non-text (e.g. an image or a table).

Blank  
This layout is used for any slides which only contain blank content, e.g. a slide containing only speaker notes, or a slide containing only a non-breaking space.

Title and Content  
This layout is used for all slides which do not match the criteria for another layout.

These layouts are chosen from the default pptx reference doc included with pandoc, unless an alternative reference doc is specified using <span class="nowrap">`--reference-doc`</span>.

<a href="#incremental-lists" class="anchor"></a>Incremental lists <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------------

By default, these writers produce lists that display “all at once.” If you want your lists to display incrementally (one item at a time), use the <a href="#option--incremental%5B" class="option"><span class="nowrap"><code>-i</code></span></a> option. If you want a particular list to depart from the default, put it in a <span class="nowrap">`div`</span> block with class <span class="nowrap">`incremental`</span> or <span class="nowrap">`nonincremental`</span>. So, for example, using the <span class="nowrap">`fenced div`</span> syntax, the following would be incremental regardless of the document default:

    ::: incremental

    - Eat spaghetti
    - Drink wine

    :::

or

    ::: nonincremental

    - Eat spaghetti
    - Drink wine

    :::

While using <span class="nowrap">`incremental`</span> and <span class="nowrap">`nonincremental`</span> divs is the recommended method of setting incremental lists on a per-case basis, an older method is also supported: putting lists inside a blockquote will depart from the document default (that is, it will display incrementally without the <a href="#option--incremental%5B" class="option"><span class="nowrap"><code>-i</code></span></a> option and all at once with the <a href="#option--incremental%5B" class="option"><span class="nowrap"><code>-i</code></span></a> option):

    > - Eat spaghetti
    > - Drink wine

Both methods allow incremental and nonincremental lists to be mixed in a single document.

If you want to include a block-quoted list, you can work around this behavior by putting the list inside a fenced div, so that it is not the direct child of the block quote:

    > ::: wrapper
    > - a
    > - list in a quote
    > :::

<a href="#inserting-pauses" class="anchor"></a>Inserting pauses <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------------

You can add “pauses” within a slide by including a paragraph containing three dots, separated by spaces:

    # Slide with a pause

    content before the pause

    . . .

    content after the pause

Note: this feature is not yet implemented for PowerPoint output.

<a href="#styling-the-slides" class="anchor"></a>Styling the slides <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------------

You can change the style of HTML slides by putting customized CSS files in <span class="nowrap">`$DATADIR/s5/default`</span> (for S5), <span class="nowrap">`$DATADIR/slidy`</span> (for Slidy), or <span class="nowrap">`$DATADIR/slideous`</span> (for Slideous), where <span class="nowrap">`$DATADIR`</span> is the user data directory (see <a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>, above). The originals may be found in pandoc’s system data directory (generally <span class="nowrap">`$CABALDIR/pandoc-VERSION/s5/default`</span>). Pandoc will look there for any files it does not find in the user data directory.

For dzslides, the CSS is included in the HTML file itself, and may be modified there.

All [reveal.js configuration options](https://revealjs.com/config/) can be set through variables. For example, themes can be used by setting the <span class="nowrap">`theme`</span> variable:

    -V theme=moon

Or you can specify a custom stylesheet using the <a href="#option--css" class="option"><span class="nowrap"><code>--css</code></span></a> option.

To style beamer slides, you can specify a <span class="nowrap">`theme`</span>, <span class="nowrap">`colortheme`</span>, <span class="nowrap">`fonttheme`</span>, <span class="nowrap">`innertheme`</span>, and <span class="nowrap">`outertheme`</span>, using the <a href="#option--variable" class="option"><span class="nowrap"><code>-V</code></span></a> option:

    pandoc -t beamer habits.txt -V theme:Warsaw -o habits.pdf

Note that heading attributes will turn into slide attributes (on a <span class="nowrap">`<div>`</span> or <span class="nowrap">`<section>`</span>) in HTML slide formats, allowing you to style individual slides. In beamer, a number of heading classes and attributes are recognized as frame options and will be passed through as options to the frame: see [Frame attributes in beamer](#frame-attributes-in-beamer), below.

<a href="#speaker-notes" class="anchor"></a>Speaker notes <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------

Speaker notes are supported in reveal.js, PowerPoint (pptx), and beamer output. You can add notes to your Markdown document thus:

    ::: notes

    This is my note.

    - It can contain Markdown
    - like this list

    :::

To show the notes window in reveal.js, press <span class="nowrap">`s`</span> while viewing the presentation. Speaker notes in PowerPoint will be available, as usual, in handouts and presenter view.

Notes are not yet supported for other slide formats, but the notes will not appear on the slides themselves.

<a href="#columns" class="anchor"></a>Columns <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------

To put material in side by side columns, you can use a native div container with class <span class="nowrap">`columns`</span>, containing two or more div containers with class <span class="nowrap">`column`</span> and a <span class="nowrap">`width`</span> attribute:

    :::::::::::::: {.columns}
    ::: {.column width="40%"}
    contents...
    :::
    ::: {.column width="60%"}
    contents...
    :::
    ::::::::::::::

Note: Specifying column widths does not currently work for PowerPoint.

### <a href="#additional-columns-attributes-in-beamer" class="anchor"></a>Additional columns attributes in beamer <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

The div containers with classes <span class="nowrap">`columns`</span> and <span class="nowrap">`column`</span> can optionally have an <span class="nowrap">`align`</span> attribute. The class <span class="nowrap">`columns`</span> can optionally have a <span class="nowrap">`totalwidth`</span> attribute or an <span class="nowrap">`onlytextwidth`</span> class.

    :::::::::::::: {.columns align=center totalwidth=8em}
    ::: {.column width="40%"}
    contents...
    :::
    ::: {.column width="60%" align=bottom}
    contents...
    :::
    ::::::::::::::

The <span class="nowrap">`align`</span> attributes on <span class="nowrap">`columns`</span> and <span class="nowrap">`column`</span> can be used with the values <span class="nowrap">`top`</span>, <span class="nowrap">`top-baseline`</span>, <span class="nowrap">`center`</span> and <span class="nowrap">`bottom`</span> to vertically align the columns. It defaults to <span class="nowrap">`top`</span> in <span class="nowrap">`columns`</span>.

The <span class="nowrap">`totalwidth`</span> attribute limits the width of the columns to the given value.

    :::::::::::::: {.columns align=top .onlytextwidth}
    ::: {.column width="40%" align=center}
    contents...
    :::
    ::: {.column width="60%"}
    contents...
    :::
    ::::::::::::::

The class <span class="nowrap">`onlytextwidth`</span> sets the <span class="nowrap">`totalwidth`</span> to <span class="nowrap">`\textwidth`</span>.

See Section 12.7 of the [Beamer User’s Guide](http://mirrors.ctan.org/macros/latex/contrib/beamer/doc/beameruserguide.pdf) for more details.

<a href="#frame-attributes-in-beamer" class="anchor"></a>Frame attributes in beamer <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------------------------------------------------

Sometimes it is necessary to add the LaTeX <span class="nowrap">`[fragile]`</span> option to a frame in beamer (for example, when using the <span class="nowrap">`minted`</span> environment). This can be forced by adding the <span class="nowrap">`fragile`</span> class to the heading introducing the slide:

    # Fragile slide {.fragile}

All of the other frame attributes described in Section 8.1 of the [Beamer User’s Guide](http://mirrors.ctan.org/macros/latex/contrib/beamer/doc/beameruserguide.pdf) may also be used: <span class="nowrap">`allowdisplaybreaks`</span>, <span class="nowrap">`allowframebreaks`</span>, <span class="nowrap">`b`</span>, <span class="nowrap">`c`</span>, <span class="nowrap">`s`</span>, <span class="nowrap">`t`</span>, <span class="nowrap">`environment`</span>, <span class="nowrap">`label`</span>, <span class="nowrap">`plain`</span>, <span class="nowrap">`shrink`</span>, <span class="nowrap">`standout`</span>, <span class="nowrap">`noframenumbering`</span>, <span class="nowrap">`squeeze`</span>. <span class="nowrap">`allowframebreaks`</span> is recommended especially for bibliographies, as it allows multiple slides to be created if the content overfills the frame:

    # References {.allowframebreaks}

In addition, the <span class="nowrap">`frameoptions`</span> attribute may be used to pass arbitrary frame options to a beamer slide:

    # Heading {frameoptions="squeeze,shrink,customoption=foobar"}

<a href="#background-in-reveal.js-beamer-and-pptx" class="anchor"></a>Background in reveal.js, beamer, and pptx <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Background images can be added to self-contained reveal.js slide shows, beamer slide shows, and pptx slide shows.

### <a href="#on-all-slides-beamer-reveal.js-pptx" class="anchor"></a>On all slides (beamer, reveal.js, pptx) <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

With beamer and reveal.js, the configuration option <span class="nowrap">`background-image`</span> can be used either in the YAML metadata block or as a command-line variable to get the same image on every slide.

Note that for reveal.js, the <span class="nowrap">`background-image`</span> will be used as a <span class="nowrap">`parallaxBackgroundImage`</span> (see below).

For pptx, you can use a <span class="nowrap">`--reference-doc`</span> in which background images have been set on the [relevant layouts](#powerpoint-layout-choice).

#### <a href="#parallaxbackgroundimage-reveal.js" class="anchor"></a><span class="nowrap">`parallaxBackgroundImage`</span> (reveal.js) <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

For reveal.js, there is also the reveal.js-native option <span class="nowrap">`parallaxBackgroundImage`</span>, which produces a parallax scrolling background. You must also set <span class="nowrap">`parallaxBackgroundSize`</span>, and can optionally set <span class="nowrap">`parallaxBackgroundHorizontal`</span> and <span class="nowrap">`parallaxBackgroundVertical`</span> to configure the scrolling behaviour. See the [reveal.js documentation](https://revealjs.com/backgrounds/#parallax-background) for more details about the meaning of these options.

In reveal.js’s overview mode, the parallaxBackgroundImage will show up only on the first slide.

### <a href="#on-individual-slides-reveal.js-pptx" class="anchor"></a>On individual slides (reveal.js, pptx) <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

To set an image for a particular reveal.js or pptx slide, add <span class="nowrap">`{background-image="/path/to/image"}`</span> to the first slide-level heading on the slide (which may even be empty).

As the [HTML writers pass unknown attributes through](#extension-link_attributes), other reveal.js background settings also work on individual slides, including <span class="nowrap">`background-size`</span>, <span class="nowrap">`background-repeat`</span>, <span class="nowrap">`background-color`</span>, <span class="nowrap">`transition`</span>, and <span class="nowrap">`transition-speed`</span>. (The <span class="nowrap">`data-`</span> prefix will automatically be added.)

Note: <span class="nowrap">`data-background-image`</span> is also supported in pptx for consistency with reveal.js – if <span class="nowrap">`background-image`</span> isn’t found, <span class="nowrap">`data-background-image`</span> will be checked.

### <a href="#on-the-title-slide-reveal.js-pptx" class="anchor"></a>On the title slide (reveal.js, pptx) <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

To add a background image to the automatically generated title slide for reveal.js, use the <span class="nowrap">`title-slide-attributes`</span> variable in the YAML metadata block. It must contain a map of attribute names and values. (Note that the <span class="nowrap">`data-`</span> prefix is required here, as it isn’t added automatically.)

For pptx, pass a <span class="nowrap">`--reference-doc`</span> with the background image set on the “Title Slide” layout.

### <a href="#example-reveal.js" class="anchor"></a>Example (reveal.js) <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>

    ---
    title: My Slide Show
    parallaxBackgroundImage: /path/to/my/background_image.png
    title-slide-attributes:
        data-background-image: /path/to/title_image.png
        data-background-size: contain
    ---

    ## Slide One

    Slide 1 has background_image.png as its background.

    ## {background-image="/path/to/special_image.jpg"}

    Slide 2 has a special image for its background, even though the heading has no content.

<a href="#epubs" class="anchor"></a>EPUBs <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=========================================================================================================

<a href="#epub-metadata" class="anchor"></a>EPUB Metadata <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------------

EPUB metadata may be specified using the <a href="#option--epub-metadata" class="option"><span class="nowrap"><code>--epub-metadata</code></span></a> option, but if the source document is Markdown, it is better to use a [YAML metadata block](#extension-yaml_metadata_block). Here is an example:

    ---
    title:
    - type: main
      text: My Book
    - type: subtitle
      text: An investigation of metadata
    creator:
    - role: author
      text: John Smith
    - role: editor
      text: Sarah Jones
    identifier:
    - scheme: DOI
      text: doi:10.234234.234/33
    publisher:  My Press
    rights: © 2007 John Smith, CC BY-NC
    ibooks:
      version: 1.3.4
    ...

The following fields are recognized:

<span class="nowrap">`identifier`</span>  
Either a string value or an object with fields <span class="nowrap">`text`</span> and <span class="nowrap">`scheme`</span>. Valid values for <span class="nowrap">`scheme`</span> are <span class="nowrap">`ISBN-10`</span>, <span class="nowrap">`GTIN-13`</span>, <span class="nowrap">`UPC`</span>, <span class="nowrap">`ISMN-10`</span>, <span class="nowrap">`DOI`</span>, <span class="nowrap">`LCCN`</span>, <span class="nowrap">`GTIN-14`</span>, <span class="nowrap">`ISBN-13`</span>, <span class="nowrap">`Legal deposit number`</span>, <span class="nowrap">`URN`</span>, <span class="nowrap">`OCLC`</span>, <span class="nowrap">`ISMN-13`</span>, <span class="nowrap">`ISBN-A`</span>, <span class="nowrap">`JP`</span>, <span class="nowrap">`OLCC`</span>.

<span class="nowrap">`title`</span>  
Either a string value, or an object with fields <span class="nowrap">`file-as`</span> and <span class="nowrap">`type`</span>, or a list of such objects. Valid values for <span class="nowrap">`type`</span> are <span class="nowrap">`main`</span>, <span class="nowrap">`subtitle`</span>, <span class="nowrap">`short`</span>, <span class="nowrap">`collection`</span>, <span class="nowrap">`edition`</span>, <span class="nowrap">`extended`</span>.

<span class="nowrap">`creator`</span>  
Either a string value, or an object with fields <span class="nowrap">`role`</span>, <span class="nowrap">`file-as`</span>, and <span class="nowrap">`text`</span>, or a list of such objects. Valid values for <span class="nowrap">`role`</span> are [MARC relators](https://loc.gov/marc/relators/relaterm.html), but pandoc will attempt to translate the human-readable versions (like “author” and “editor”) to the appropriate marc relators.

<span class="nowrap">`contributor`</span>  
Same format as <span class="nowrap">`creator`</span>.

<span class="nowrap">`date`</span>  
A string value in <span class="nowrap">`YYYY-MM-DD`</span> format. (Only the year is necessary.) Pandoc will attempt to convert other common date formats.

<span class="nowrap">`lang`</span> (or legacy: <span class="nowrap">`language`</span>)  
A string value in [BCP 47](https://tools.ietf.org/html/bcp47) format. Pandoc will default to the local language if nothing is specified.

<span class="nowrap">`subject`</span>  
Either a string value, or an object with fields <span class="nowrap">`text`</span>, <span class="nowrap">`authority`</span>, and <span class="nowrap">`term`</span>, or a list of such objects. Valid values for <span class="nowrap">`authority`</span> are either a [reserved authority value](https://idpf.github.io/epub-registries/authorities/) (currently <span class="nowrap">`AAT`</span>, <span class="nowrap">`BIC`</span>, <span class="nowrap">`BISAC`</span>, <span class="nowrap">`CLC`</span>, <span class="nowrap">`DDC`</span>, <span class="nowrap">`CLIL`</span>, <span class="nowrap">`EuroVoc`</span>, <span class="nowrap">`MEDTOP`</span>, <span class="nowrap">`LCSH`</span>, <span class="nowrap">`NDC`</span>, <span class="nowrap">`Thema`</span>, <span class="nowrap">`UDC`</span>, and <span class="nowrap">`WGS`</span>) or an absolute IRI identifying a custom scheme. Valid values for <span class="nowrap">`term`</span> are defined by the scheme.

<span class="nowrap">`description`</span>  
A string value.

<span class="nowrap">`type`</span>  
A string value.

<span class="nowrap">`format`</span>  
A string value.

<span class="nowrap">`relation`</span>  
A string value.

<span class="nowrap">`coverage`</span>  
A string value.

<span class="nowrap">`rights`</span>  
A string value.

<span class="nowrap">`belongs-to-collection`</span>  
A string value. Identifies the name of a collection to which the EPUB Publication belongs.

<span class="nowrap">`group-position`</span>  
The <span class="nowrap">`group-position`</span> field indicates the numeric position in which the EPUB Publication belongs relative to other works belonging to the same <span class="nowrap">`belongs-to-collection`</span> field.

<span class="nowrap">`cover-image`</span>  
A string value (path to cover image).

<span class="nowrap">`css`</span> (or legacy: <span class="nowrap">`stylesheet`</span>)  
A string value (path to CSS stylesheet).

<span class="nowrap">`page-progression-direction`</span>  
Either <span class="nowrap">`ltr`</span> or <span class="nowrap">`rtl`</span>. Specifies the <span class="nowrap">`page-progression-direction`</span> attribute for the [<span class="nowrap">`spine`</span> element](http://idpf.org/epub/301/spec/epub-publications.html#sec-spine-elem).

<span class="nowrap">`accessModes`</span>  
An array of strings ([schema](https://kb.daisy.org/publishing/docs/metadata/schema.org/index.html)). Defaults to <span class="nowrap">`["textual"]`</span>.

<span class="nowrap">`accessModeSufficient`</span>  
An array of strings ([schema](https://kb.daisy.org/publishing/docs/metadata/schema.org/index.html)). Defaults to <span class="nowrap">`["textual"]`</span>.

<span class="nowrap">`accessibilityHazards`</span>  
An array of strings ([schema](https://kb.daisy.org/publishing/docs/metadata/schema.org/index.html)). Defaults to <span class="nowrap">`["none"]`</span>.

<span class="nowrap">`accessibilityFeatures`</span>  
An array of strings ([schema](https://kb.daisy.org/publishing/docs/metadata/schema.org/index.html)). Defaults to

    - "alternativeText"
    - "readingOrder"
    - "structuralNavigation"
    - "tableOfContents"

<span class="nowrap">`accessibilitySummary`</span>  
A string value.

<span class="nowrap">`ibooks`</span>  
iBooks-specific metadata, with the following fields:

-   <span class="nowrap">`version`</span>: (string)
-   <span class="nowrap">`specified-fonts`</span>: <span class="nowrap">`true`</span>|<span class="nowrap">`false`</span> (default <span class="nowrap">`false`</span>)
-   <span class="nowrap">`ipad-orientation-lock`</span>: <span class="nowrap">`portrait-only`</span>|<span class="nowrap">`landscape-only`</span>
-   <span class="nowrap">`iphone-orientation-lock`</span>: <span class="nowrap">`portrait-only`</span>|<span class="nowrap">`landscape-only`</span>
-   <span class="nowrap">`binding`</span>: <span class="nowrap">`true`</span>|<span class="nowrap">`false`</span> (default <span class="nowrap">`true`</span>)
-   <span class="nowrap">`scroll-axis`</span>: <span class="nowrap">`vertical`</span>|<span class="nowrap">`horizontal`</span>|<span class="nowrap">`default`</span>

<a href="#the-epubtype-attribute" class="anchor"></a>The <span class="nowrap">`epub:type`</span> attribute <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------

For <span class="nowrap">`epub3`</span> output, you can mark up the heading that corresponds to an EPUB chapter using the [<span class="nowrap">`epub:type`</span> attribute](http://www.idpf.org/epub/31/spec/epub-contentdocs.html#sec-epub-type-attribute). For example, to set the attribute to the value <span class="nowrap">`prologue`</span>, use this Markdown:

    # My chapter {epub:type=prologue}

Which will result in:

    <body epub:type="frontmatter">
      <section epub:type="prologue">
        <h1>My chapter</h1>

Pandoc will output <span class="nowrap">`<body epub:type="bodymatter">`</span>, unless you use one of the following values, in which case either <span class="nowrap">`frontmatter`</span> or <span class="nowrap">`backmatter`</span> will be output.

<table><thead><tr class="header"><th><span class="nowrap"><code>epub:type</code></span> of first section</th><th><span class="nowrap"><code>epub:type</code></span> of body</th></tr></thead><tbody><tr class="odd"><td>prologue</td><td>frontmatter</td></tr><tr class="even"><td>abstract</td><td>frontmatter</td></tr><tr class="odd"><td>acknowledgments</td><td>frontmatter</td></tr><tr class="even"><td>copyright-page</td><td>frontmatter</td></tr><tr class="odd"><td>dedication</td><td>frontmatter</td></tr><tr class="even"><td>credits</td><td>frontmatter</td></tr><tr class="odd"><td>keywords</td><td>frontmatter</td></tr><tr class="even"><td>imprint</td><td>frontmatter</td></tr><tr class="odd"><td>contributors</td><td>frontmatter</td></tr><tr class="even"><td>other-credits</td><td>frontmatter</td></tr><tr class="odd"><td>errata</td><td>frontmatter</td></tr><tr class="even"><td>revision-history</td><td>frontmatter</td></tr><tr class="odd"><td>titlepage</td><td>frontmatter</td></tr><tr class="even"><td>halftitlepage</td><td>frontmatter</td></tr><tr class="odd"><td>seriespage</td><td>frontmatter</td></tr><tr class="even"><td>foreword</td><td>frontmatter</td></tr><tr class="odd"><td>preface</td><td>frontmatter</td></tr><tr class="even"><td>frontispiece</td><td>frontmatter</td></tr><tr class="odd"><td>appendix</td><td>backmatter</td></tr><tr class="even"><td>colophon</td><td>backmatter</td></tr><tr class="odd"><td>bibliography</td><td>backmatter</td></tr><tr class="even"><td>index</td><td>backmatter</td></tr></tbody></table>

<a href="#linked-media" class="anchor"></a>Linked media <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------

By default, pandoc will download media referenced from any <span class="nowrap">`<img>`</span>, <span class="nowrap">`<audio>`</span>, <span class="nowrap">`<video>`</span> or <span class="nowrap">`<source>`</span> element present in the generated EPUB, and include it in the EPUB container, yielding a completely self-contained EPUB. If you want to link to external media resources instead, use raw HTML in your source and add <span class="nowrap">`data-external="1"`</span> to the tag with the <span class="nowrap">`src`</span> attribute. For example:

    <audio controls="1">
      <source src="https://example.com/music/toccata.mp3"
              data-external="1" type="audio/mpeg">
      </source>
    </audio>

If the input format already is HTML then <span class="nowrap">`data-external="1"`</span> will work as expected for <span class="nowrap">`<img>`</span> elements. Similarly, for Markdown, external images can be declared with <span class="nowrap">`![img](url){external=1}`</span>. Note that this only works for images; the other media elements have no native representation in pandoc’s AST and require the use of raw HTML.

<a href="#epub-styling" class="anchor"></a>EPUB styling <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------

By default, pandoc will include some basic styling contained in its <span class="nowrap">`epub.css`</span> data file. (To see this, use <span class="nowrap">`pandoc --print-default-data-file epub.css`</span>.) To use a different CSS file, just use the <a href="#option--css" class="option"><span class="nowrap"><code>--css</code></span></a> command line option. A few inline styles are defined in addition; these are essential for correct formatting of pandoc’s HTML output.

The <span class="nowrap">`document-css`</span> variable may be set if the more opinionated styling of pandoc’s default HTML templates is desired (and in that case the variables defined in [Variables for HTML](#variables-for-html) may be used to fine-tune the style).

<a href="#chunked-html" class="anchor"></a>Chunked HTML <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=======================================================================================================================

<span class="nowrap">`pandoc -t chunkedhtml`</span> will produce a zip archive of linked HTML files, one for each section of the original document. Internal links will automatically be adjusted to point to the right place, images linked to under the working directory will be incorporated, and navigation links will be added. In addition, a JSON file <span class="nowrap">`sitemap.json`</span> will be included describing the hierarchical structure of the files.

If an output file without an extension is specified, then it will be interpreted as a directory and the zip archive will be automatically unpacked into it (unless it already exists, in which case an error will be raised). Otherwise a <span class="nowrap">`.zip`</span> file will be produced.

The navigation links can be customized by adjusting the template. By default, a table of contents is included only on the top page. To include it on every page, set the <span class="nowrap">`toc`</span> variable manually.

<a href="#jupyter-notebooks" class="anchor"></a>Jupyter notebooks <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=================================================================================================================================

When creating a [Jupyter notebook](https://nbformat.readthedocs.io/en/latest/), pandoc will try to infer the notebook structure. Code blocks with the class <span class="nowrap">`code`</span> will be taken as code cells, and intervening content will be taken as Markdown cells. Attachments will automatically be created for images in Markdown cells. Metadata will be taken from the <span class="nowrap">`jupyter`</span> metadata field. For example:

    ---
    title: My notebook
    jupyter:
      nbformat: 4
      nbformat_minor: 5
      kernelspec:
         display_name: Python 2
         language: python
         name: python2
      language_info:
         codemirror_mode:
           name: ipython
           version: 2
         file_extension: ".py"
         mimetype: "text/x-python"
         name: "python"
         nbconvert_exporter: "python"
         pygments_lexer: "ipython2"
         version: "2.7.15"
    ---

    # Lorem ipsum

    **Lorem ipsum** dolor sit amet, consectetur adipiscing elit. Nunc luctus
    bibendum felis dictum sodales.

    ``` code
    print("hello")
    ```

    ## Pyout

    ``` code
    from IPython.display import HTML
    HTML("""
    <script>
    console.log("hello");
    </script>
    <b>HTML</b>
    """)
    ```

    ## Image

    This image ![image](myimage.png) will be
    included as a cell attachment.

If you want to add cell attributes, group cells differently, or add output to code cells, then you need to include divs to indicate the structure. You can use either [fenced divs](#extension-fenced_divs) or [native divs](#extension-native_divs) for this. Here is an example:

    :::::: {.cell .markdown}
    # Lorem

    **Lorem ipsum** dolor sit amet, consectetur adipiscing elit. Nunc luctus
    bibendum felis dictum sodales.
    ::::::

    :::::: {.cell .code execution_count=1}
    ``` {.python}
    print("hello")
    ```

    ::: {.output .stream .stdout}
    ```
    hello
    ```
    :::
    ::::::

    :::::: {.cell .code execution_count=2}
    ``` {.python}
    from IPython.display import HTML
    HTML("""
    <script>
    console.log("hello");
    </script>
    <b>HTML</b>
    """)
    ```

    ::: {.output .execute_result execution_count=2}
    ```{=html}
    <script>
    console.log("hello");
    </script>
    <b>HTML</b>
    hello
    ```
    :::
    ::::::

If you include raw HTML or TeX in an output cell, use the [raw attribute](#extension-raw_attribute), as shown in the last cell of the example above. Although pandoc can process “bare” raw HTML and TeX, the result is often interspersed raw elements and normal textual elements, and in an output cell pandoc expects a single, connected raw block. To avoid using raw HTML or TeX except when marked explicitly using raw attributes, we recommend specifying the extensions <span class="nowrap">`-raw_html-raw_tex+raw_attribute`</span> when translating between Markdown and ipynb notebooks.

Note that options and extensions that affect reading and writing of Markdown will also affect Markdown cells in ipynb notebooks. For example, <a href="#option--wrap" class="option"><span class="nowrap"><code>--wrap=preserve</code></span></a> will preserve soft line breaks in Markdown cells; <a href="#option--markdown-headings" class="option"><span class="nowrap"><code>--markdown-headings=setext</code></span></a> will cause Setext-style headings to be used; and <a href="#option--preserve-tabs%5B" class="option"><span class="nowrap"><code>--preserve-tabs</code></span></a> will prevent tabs from being turned to spaces.

<a href="#syntax-highlighting" class="anchor"></a>Syntax highlighting <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=====================================================================================================================================

Pandoc will automatically highlight syntax in [fenced code blocks](#fenced-code-blocks) that are marked with a language name. The Haskell library [skylighting](https://github.com/jgm/skylighting) is used for highlighting. Currently highlighting is supported only for HTML, EPUB, Docx, Ms, Man, and LaTeX/PDF output. To see a list of language names that pandoc will recognize, type <span class="nowrap">`pandoc --list-highlight-languages`</span>.

The color scheme can be selected using the <a href="#option--highlight-style" class="option"><span class="nowrap"><code>--highlight-style</code></span></a> option. The default color scheme is <span class="nowrap">`pygments`</span>, which imitates the default color scheme used by the Python library pygments (though pygments is not actually used to do the highlighting). To see a list of highlight styles, type <span class="nowrap">`pandoc --list-highlight-styles`</span>.

If you are not satisfied with the predefined styles, you can use <a href="#option--print-highlight-style" class="option"><span class="nowrap"><code>--print-highlight-style</code></span></a> to generate a JSON <span class="nowrap">`.theme`</span> file which can be modified and used as the argument to <a href="#option--highlight-style" class="option"><span class="nowrap"><code>--highlight-style</code></span></a>. To get a JSON version of the <span class="nowrap">`pygments`</span> style, for example:

    pandoc -o my.theme --print-highlight-style pygments

Then edit <span class="nowrap">`my.theme`</span> and use it like this:

    pandoc --highlight-style my.theme

If you are not satisfied with the built-in highlighting, or you want to highlight a language that isn’t supported, you can use the <a href="#option--syntax-definition" class="option"><span class="nowrap"><code>--syntax-definition</code></span></a> option to load a [KDE-style XML syntax definition file](https://docs.kde.org/stable5/en/kate/katepart/highlight.html). Before writing your own, have a look at KDE’s [repository of syntax definitions](https://github.com/KDE/syntax-highlighting/tree/master/data/syntax).

If you receive an error that pandoc “Could not read highlighting theme”, check that the JSON file is encoded with UTF-8 and has no Byte-Order Mark (BOM).

To disable highlighting, use the <a href="#option--no-highlight" class="option"><span class="nowrap"><code>--no-highlight</code></span></a> option.

<a href="#custom-styles" class="anchor"></a>Custom Styles <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=========================================================================================================================

Custom styles can be used in the docx, odt and ICML formats.

<a href="#output" class="anchor"></a>Output <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------

By default, pandoc’s odt, docx and ICML output applies a predefined set of styles for blocks such as paragraphs and block quotes, and uses largely default formatting (italics, bold) for inlines. This will work for most purposes, especially alongside a [reference doc](#option--reference-doc) file. However, if you need to apply your own styles to blocks, or match a preexisting set of styles, pandoc allows you to define custom styles for blocks and text using <span class="nowrap">`div`</span>s and <span class="nowrap">`span`</span>s, respectively.

If you define a <span class="nowrap">`div`</span> or <span class="nowrap">`span`</span> with the attribute <span class="nowrap">`custom-style`</span>, pandoc will apply your specified style to the contained elements (with the exception of elements whose function depends on a style, like headings, code blocks, block quotes, or links). So, for example, using the <span class="nowrap">`bracketed_spans`</span> syntax,

    [Get out]{custom-style="Emphatically"}, he said.

would produce a file with “Get out” styled with character style <span class="nowrap">`Emphatically`</span>. Similarly, using the <span class="nowrap">`fenced_divs`</span> syntax,

    Dickinson starts the poem simply:

    ::: {custom-style="Poetry"}
    | A Bird came down the Walk---
    | He did not know I saw---
    :::

would style the two contained lines with the <span class="nowrap">`Poetry`</span> paragraph style.

Styles will be defined in the output file as inheriting from normal text (docx) or Default Paragraph Style (odt), if the styles are not yet in your [reference doc](#option--reference-doc). If they are already defined, pandoc will not alter the definition.

This feature allows for greatest customization in conjunction with [pandoc filters](https://pandoc.org/filters.html). If you want all paragraphs after block quotes to be indented, you can write a filter to apply the styles necessary. If you want all italics to be transformed to the <span class="nowrap">`Emphasis`</span> character style (perhaps to change their color), you can write a filter which will transform all italicized inlines to inlines within an <span class="nowrap">`Emphasis`</span> custom-style <span class="nowrap">`span`</span>.

For docx or odt output, you don’t need to enable any extensions for custom styles to work.

<a href="#input" class="anchor"></a>Input <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
---------------------------------------------------------------------------------------------------------

The docx reader, by default, only reads those styles that it can convert into pandoc elements, either by direct conversion or interpreting the derivation of the input document’s styles.

By enabling the [<span class="nowrap">`styles`</span> extension](#ext-styles) in the docx reader (<a href="#option--from" class="option"><span class="nowrap"><code>-f docx+styles</code></span></a>), you can produce output that maintains the styles of the input document, using the <span class="nowrap">`custom-style`</span> class. Paragraph styles are interpreted as divs, while character styles are interpreted as spans.

For example, using the <span class="nowrap">`custom-style-reference.docx`</span> file in the test directory, we have the following different outputs:

Without the <span class="nowrap">`+styles`</span> extension:

    $ pandoc test/docx/custom-style-reference.docx -f docx -t markdown
    This is some text.

    This is text with an *emphasized* text style. And this is text with a
    **strengthened** text style.

    > Here is a styled paragraph that inherits from Block Text.

And with the extension:

    $ pandoc test/docx/custom-style-reference.docx -f docx+styles -t markdown

    ::: {custom-style="First Paragraph"}
    This is some text.
    :::

    ::: {custom-style="Body Text"}
    This is text with an [emphasized]{custom-style="Emphatic"} text style.
    And this is text with a [strengthened]{custom-style="Strengthened"}
    text style.
    :::

    ::: {custom-style="My Block Style"}
    > Here is a styled paragraph that inherits from Block Text.
    :::

With these custom styles, you can use your input document as a reference-doc while creating docx output (see below), and maintain the same styles in your input and output files.

<a href="#custom-readers-and-writers" class="anchor"></a>Custom readers and writers <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
===================================================================================================================================================

Pandoc can be extended with custom readers and writers written in [Lua](https://www.lua.org). (Pandoc includes a Lua interpreter, so Lua need not be installed separately.)

To use a custom reader or writer, simply specify the path to the Lua script in place of the input or output format. For example:

    pandoc -t data/sample.lua
    pandoc -f my_custom_markup_language.lua -t latex -s

If the script is not found relative to the working directory, it will be sought in the <span class="nowrap">`custom`</span> subdirectory of the user data directory (see <a href="#option--data-dir" class="option"><span class="nowrap"><code>--data-dir</code></span></a>).

A custom reader is a Lua script that defines one function, Reader, which takes a string as input and returns a Pandoc AST. See the [Lua filters documentation](https://pandoc.org/lua-filters.html) for documentation of the functions that are available for creating pandoc AST elements. For parsing, the [lpeg](http://www.inf.puc-rio.br/~roberto/lpeg/) parsing library is available by default. To see a sample custom reader:

    pandoc --print-default-data-file creole.lua

If you want your custom reader to have access to reader options (e.g. the tab stop setting), you give your Reader function a second <span class="nowrap">`options`</span> parameter.

A custom writer is a Lua script that defines a function that specifies how to render each element in a Pandoc AST. See the [djot-writer.lua](https://github.com/jgm/djot.lua/blob/main/djot-writer.lua) for a full-featured example.

Note that custom writers have no default template. If you want to use <a href="#option--standalone" class="option"><span class="nowrap"><code>--standalone</code></span></a> with a custom writer, you will need to specify a template manually using <a href="#option--template" class="option"><span class="nowrap"><code>--template</code></span></a> or add a new default template with the name <span class="nowrap">`default.NAME_OF_CUSTOM_WRITER.lua`</span> to the <span class="nowrap">`templates`</span> subdirectory of your user data directory (see [Templates](#templates)).

<a href="#reproducible-builds" class="anchor"></a>Reproducible builds <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=====================================================================================================================================

Some of the document formats pandoc targets (such as EPUB, docx, and ODT) include build timestamps in the generated document. That means that the files generated on successive builds will differ, even if the source does not. To avoid this, set the <span class="nowrap">`SOURCE_DATE_EPOCH`</span> environment variable, and the timestamp will be taken from it instead of the current time. <span class="nowrap">`SOURCE_DATE_EPOCH`</span> should contain an integer unix timestamp (specifying the number of seconds since midnight UTC January 1, 1970).

Some document formats also include a unique identifier. For EPUB, this can be set explicitly by setting the <span class="nowrap">`identifier`</span> metadata field (see [EPUB Metadata](#epub-metadata), above).

<a href="#accessible-pdfs-and-pdf-archiving-standards" class="anchor"></a>Accessible PDFs and PDF archiving standards <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=====================================================================================================================================================================================

PDF is a flexible format, and using PDF in certain contexts requires additional conventions. For example, PDFs are not accessible by default; they define how characters are placed on a page but do not contain semantic information on the content. However, it is possible to generate accessible PDFs, which use tagging to add semantic information to the document.

Pandoc defaults to LaTeX to generate PDF. Tagging support in LaTeX is in development and not readily available, so PDFs generated in this way will always be untagged and not accessible. This means that alternative engines must be used to generate accessible PDFs.

The PDF standards PDF/A and PDF/UA define further restrictions intended to optimize PDFs for archiving and accessibility. Tagging is commonly used in combination with these standards to ensure best results.

Note, however, that standard compliance depends on many things, including the colorspace of embedded images. Pandoc cannot check this, and external programs must be used to ensure that generated PDFs are in compliance.

<a href="#context" class="anchor"></a>ConTeXt <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------

ConTeXt always produces tagged PDFs, but the quality depends on the input. The default ConTeXt markup generated by pandoc is optimized for readability and reuse, not tagging. Enable the [<span class="nowrap">`tagging`</span>](#extension--tagging) format extension to force markup that is optimized for tagging. This can be combined with the <span class="nowrap">`pdfa`</span> variable to generate standard-compliant PDFs. E.g.:

    pandoc --to=context+tagging -V pdfa=3a

A recent <span class="nowrap">`context`</span> version should be used, as older versions contained a bug that lead to invalid PDF metadata.

<a href="#weasyprint" class="anchor"></a>WeasyPrint <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------

The HTML-based engine WeasyPrint includes experimental support for PDF/A and PDF/UA since version 57. Tagged PDFs can created with

    pandoc --pdf-engine=weasyprint \
           --pdf-engine-opt=--pdf-variant=pdf/ua-1 ...

The feature is experimental and standard compliance should not be assumed.

<a href="#prince-xml" class="anchor"></a>Prince XML <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-------------------------------------------------------------------------------------------------------------------

The non-free HTML-to-PDf converter <span class="nowrap">`prince`</span> has extensive support for various PDF standards as well as tagging. E.g.:

    pandoc --pdf-engine=prince \
           --pdf-engine-opt=--tagged-pdf ...

See the prince documentation for more info.

<a href="#word-processors" class="anchor"></a>Word Processors <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
-----------------------------------------------------------------------------------------------------------------------------

Word processors like LibreOffice and MS Word can also be used to generate standardized and tagged PDF output. Pandoc does not support direct conversions via these tools. However, pandoc can convert a document to a <span class="nowrap">`docx`</span> or <span class="nowrap">`odt`</span> file, which can then be opened and converted to PDF with the respective word processor. See the documentation for [Word](https://support.microsoft.com/en-us/office/create-accessible-pdfs-064625e0-56ea-4e16-ad71-3aa33bb4b7ed) and [LibreOffice](https://help.libreoffice.org/7.1/en-US/text/shared/01/ref_pdf_export_general.html).

<a href="#running-pandoc-as-a-web-server" class="anchor"></a>Running pandoc as a web server <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
===========================================================================================================================================================

If you rename (or symlink) the pandoc executable to <span class="nowrap">`pandoc-server`</span>, or if you call pandoc with <span class="nowrap">`server`</span> as the first argument, it will start up a web server with a JSON API. This server exposes most of the conversion functionality of pandoc. For full documentation, see the [pandoc-server](https://github.com/jgm/pandoc/blob/master/doc/pandoc-server.md) man page.

If you rename (or symlink) the pandoc executable to <span class="nowrap">`pandoc-server.cgi`</span>, it will function as a CGI program exposing the same API as <span class="nowrap">`pandoc-server`</span>.

<span class="nowrap">`pandoc-server`</span> is designed to be maximally secure; it uses Haskell’s type system to provide strong guarantees that no I/O will be performed on the server during pandoc conversions.

<a href="#running-pandoc-as-a-lua-interpreter" class="anchor"></a>Running pandoc as a Lua interpreter <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=====================================================================================================================================================================

Calling the pandoc executable under the name <span class="nowrap">`pandoc-lua`</span> or with <span class="nowrap">`lua`</span> as the first argument will make it function as a standalone Lua interpreter. The behavior is mostly identical to that of the [standalone <span class="nowrap">`lua`</span> executable](https://www.lua.org/manual/5.4/manual.html#7), version 5.4. For full documentation, see the [pandoc-lua](https://github.com/jgm/pandoc/blob/master/doc/pandoc-lua.md) man page.

<a href="#a-note-on-security" class="anchor"></a>A note on security <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
===================================================================================================================================

1.  Although pandoc itself will not create or modify any files other than those you explicitly ask it create (with the exception of temporary files used in producing PDFs), a filter or custom writer could in principle do anything on your file system. Please audit filters and custom writers very carefully before using them.

2.  Several input formats (including HTML, Org, and RST) support <span class="nowrap">`include`</span> directives that allow the contents of a file to be included in the output. An untrusted attacker could use these to view the contents of files on the file system. (Using the <a href="#option--sandbox%5B" class="option"><span class="nowrap"><code>--sandbox</code></span></a> option can protect against this threat.)

3.  Several output formats (including RTF, FB2, HTML with <a href="#option--self-contained%5B" class="option"><span class="nowrap"><code>--self-contained</code></span></a>, EPUB, Docx, and ODT) will embed encoded or raw images into the output file. An untrusted attacker could exploit this to view the contents of non-image files on the file system. (Using the <a href="#option--sandbox%5B" class="option"><span class="nowrap"><code>--sandbox</code></span></a> option can protect against this threat, but will also prevent including images in these formats.)

4.  If your application uses pandoc as a Haskell library (rather than shelling out to the executable), it is possible to use it in a mode that fully isolates pandoc from your file system, by running the pandoc operations in the <span class="nowrap">`PandocPure`</span> monad. See the document [Using the pandoc API](https://pandoc.org/using-the-pandoc-api.html) for more details. (This corresponds to the use of the <a href="#option--sandbox%5B" class="option"><span class="nowrap"><code>--sandbox</code></span></a> option on the command line.)

5.  Pandoc’s parsers can exhibit pathological performance on some corner cases. It is wise to put any pandoc operations under a timeout, to avoid DOS attacks that exploit these issues. If you are using the pandoc executable, you can add the command line options <span class="nowrap">`+RTS -M512M -RTS`</span> (for example) to limit the heap size to 512MB. Note that the <span class="nowrap">`commonmark`</span> parser (including <span class="nowrap">`commonmark_x`</span> and <span class="nowrap">`gfm`</span>) is much less vulnerable to pathological performance than the <span class="nowrap">`markdown`</span> parser, so it is a better choice when processing untrusted input.

6.  The HTML generated by pandoc is not guaranteed to be safe. If <span class="nowrap">`raw_html`</span> is enabled for the Markdown input, users can inject arbitrary HTML. Even if <span class="nowrap">`raw_html`</span> is disabled, users can include dangerous content in URLs and attributes. To be safe, you should run all HTML generated from untrusted user input through an HTML sanitizer.

<a href="#authors" class="anchor"></a>Authors <span class="extension-checkbox" title="enabled by default for:
      + markdown
      + opml
      + org
      + typst

      disabled by default for:
      - docx
      - ipynb
      - markdown_github
      - markdown_mmd
      - markdown_phpextra
      - markdown_strict
      - plain
      " aria-hidden="true">±</span>
=============================================================================================================

Copyright 2006–2024 John MacFarlane (<a href="/cdn-cgi/l/email-protection" class="__cf_email__">[email protected]</a>). Released under the [GPL](https://www.gnu.org/copyleft/gpl.html "GNU General Public License"), version 2 or greater. This software carries no warranty of any kind. (See COPYRIGHT for full copyright and warranty notices.) For a full list of contributors, see the file AUTHORS.md in the pandoc source code.

------------------------------------------------------------------------

1.  The point of this rule is to ensure that normal paragraphs starting with people’s initials, like

        B. Russell won a Nobel Prize (but not for "On Denoting").

    do not get treated as list items.

    This rule will not prevent

        (C) 2007 Joe Smith

    from being interpreted as a list item. In this case, a backslash escape can be used:

        (C\) 2007 Joe Smith

    <a href="#fnref1" class="footnote-back">↩︎</a>

2.  I have been influenced by the suggestions of [David Wheeler](https://justatheory.com/2009/02/modest-markdown-proposal/).<a href="#fnref2" class="footnote-back">↩︎</a>

3.  This scheme is due to Michel Fortin, who proposed it on the [Markdown discussion list](http://six.pairlist.net/pipermail/markdown-discuss/2005-March/001097.html).<a href="#fnref3" class="footnote-back">↩︎</a>

4.  To see why laziness is incompatible with relaxing the requirement of a blank line between items, consider the following example:

        bar
        :    definition
        foo
        :    definition

    Is this a single list item with two definitions of “bar,” the first of which is lazily wrapped, or two list items? To remove the ambiguity we must either disallow lazy wrapping or require a blank line between list items.<a href="#fnref4" class="footnote-back">↩︎</a>

-   <a href="#synopsis" id="toc-synopsis">Synopsis <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
-   <a href="#description" id="toc-description">Description <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
    -   <a href="#using-pandoc" id="toc-using-pandoc">Using pandoc <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#specifying-formats" id="toc-specifying-formats">Specifying formats <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#character-encoding" id="toc-character-encoding">Character encoding <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#creating-a-pdf" id="toc-creating-a-pdf">Creating a PDF <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#reading-from-the-web" id="toc-reading-from-the-web">Reading from the Web <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
-   <a href="#options" id="toc-options">Options <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
    -   <a href="#general-options" id="toc-general-options">General options <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#reader-options" id="toc-reader-options">Reader options <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#general-writer-options" id="toc-general-writer-options">General writer options <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#options-affecting-specific-writers" id="toc-options-affecting-specific-writers">Options affecting specific writers <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#citation-rendering" id="toc-citation-rendering">Citation rendering <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#math-rendering-in-html" id="toc-math-rendering-in-html">Math rendering in HTML <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#options-for-wrapper-scripts" id="toc-options-for-wrapper-scripts">Options for wrapper scripts <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
-   <a href="#exit-codes" id="toc-exit-codes">Exit codes <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
-   <a href="#defaults-files" id="toc-defaults-files">Defaults files <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
    -   <a href="#general-options-1" id="toc-general-options-1">General options <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#reader-options-1" id="toc-reader-options-1">Reader options <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#general-writer-options-1" id="toc-general-writer-options-1">General writer options <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#options-affecting-specific-writers-1" id="toc-options-affecting-specific-writers-1">Options affecting specific writers <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#citation-rendering-1" id="toc-citation-rendering-1">Citation rendering <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#math-rendering-in-html-1" id="toc-math-rendering-in-html-1">Math rendering in HTML <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#options-for-wrapper-scripts-1" id="toc-options-for-wrapper-scripts-1">Options for wrapper scripts <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
-   <a href="#templates" id="toc-templates">Templates <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
    -   <a href="#template-syntax" id="toc-template-syntax">Template syntax <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#comments" id="toc-comments">Comments <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#delimiters" id="toc-delimiters">Delimiters <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#interpolated-variables" id="toc-interpolated-variables">Interpolated variables <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#conditionals" id="toc-conditionals">Conditionals <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#for-loops" id="toc-for-loops">For loops <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#partials" id="toc-partials">Partials <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#nesting" id="toc-nesting">Nesting <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#breakable-spaces" id="toc-breakable-spaces">Breakable spaces <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#pipes" id="toc-pipes">Pipes <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#variables" id="toc-variables">Variables <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#metadata-variables" id="toc-metadata-variables">Metadata variables <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#language-variables" id="toc-language-variables">Language variables <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#variables-for-html" id="toc-variables-for-html">Variables for HTML <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#variables-for-html-math" id="toc-variables-for-html-math">Variables for HTML math <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#variables-for-html-slides" id="toc-variables-for-html-slides">Variables for HTML slides <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#variables-for-beamer-slides" id="toc-variables-for-beamer-slides">Variables for Beamer slides <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#variables-for-powerpoint" id="toc-variables-for-powerpoint">Variables for PowerPoint <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#variables-for-latex" id="toc-variables-for-latex">Variables for LaTeX <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#variables-for-context" id="toc-variables-for-context">Variables for ConTeXt <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#variables-for-wkhtmltopdf" id="toc-variables-for-wkhtmltopdf">Variables for <span class="nowrap"><code>wkhtmltopdf</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#variables-for-man-pages" id="toc-variables-for-man-pages">Variables for man pages <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#variables-for-texinfo" id="toc-variables-for-texinfo">Variables for Texinfo <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#variables-for-typst" id="toc-variables-for-typst">Variables for Typst <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#variables-for-ms" id="toc-variables-for-ms">Variables for ms <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#variables-set-automatically" id="toc-variables-set-automatically">Variables set automatically <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
-   <a href="#extensions" id="toc-extensions">Extensions <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
    -   <a href="#typography" id="toc-typography">Typography <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-smart" id="toc-extension-smart">Extension: <span class="nowrap"><code>smart</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#headings-and-sections" id="toc-headings-and-sections">Headings and sections <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-auto_identifiers" id="toc-extension-auto_identifiers">Extension: <span class="nowrap"><code>auto_identifiers</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-ascii_identifiers" id="toc-extension-ascii_identifiers">Extension: <span class="nowrap"><code>ascii_identifiers</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-gfm_auto_identifiers" id="toc-extension-gfm_auto_identifiers">Extension: <span class="nowrap"><code>gfm_auto_identifiers</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#math-input" id="toc-math-input">Math Input <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#raw-htmltex" id="toc-raw-htmltex">Raw HTML/TeX <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#literate-haskell-support" id="toc-literate-haskell-support">Literate Haskell support <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-literate_haskell" id="toc-extension-literate_haskell">Extension: <span class="nowrap"><code>literate_haskell</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#other-extensions" id="toc-other-extensions">Other extensions <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-empty_paragraphs" id="toc-extension-empty_paragraphs">Extension: <span class="nowrap"><code>empty_paragraphs</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-native_numbering" id="toc-extension-native_numbering">Extension: <span class="nowrap"><code>native_numbering</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-xrefs_name" id="toc-extension-xrefs_name">Extension: <span class="nowrap"><code>xrefs_name</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-xrefs_number" id="toc-extension-xrefs_number">Extension: <span class="nowrap"><code>xrefs_number</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#ext-styles" id="toc-ext-styles">Extension: <span class="nowrap"><code>styles</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-amuse" id="toc-extension-amuse">Extension: <span class="nowrap"><code>amuse</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-raw_markdown" id="toc-extension-raw_markdown">Extension: <span class="nowrap"><code>raw_markdown</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#typst-citations" id="toc-typst-citations">Extension: <span class="nowrap"><code>citations</code></span> (typst) <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#org-citations" id="toc-org-citations">Extension: <span class="nowrap"><code>citations</code></span> (org) <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#docx-citations" id="toc-docx-citations">Extension: <span class="nowrap"><code>citations</code></span> (docx) <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#org-fancy-lists" id="toc-org-fancy-lists">Extension: <span class="nowrap"><code>fancy_lists</code></span> (org) <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-element_citations" id="toc-extension-element_citations">Extension: <span class="nowrap"><code>element_citations</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-ntb" id="toc-extension-ntb">Extension: <span class="nowrap"><code>ntb</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension--tagging" id="toc-extension--tagging">Extension: <span class="nowrap"><code>tagging</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
-   <a href="#pandocs-markdown" id="toc-pandocs-markdown">Pandoc’s Markdown <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
    -   <a href="#philosophy" id="toc-philosophy">Philosophy <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#paragraphs" id="toc-paragraphs">Paragraphs <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-escaped_line_breaks" id="toc-extension-escaped_line_breaks">Extension: <span class="nowrap"><code>escaped_line_breaks</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#headings" id="toc-headings">Headings <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#setext-style-headings" id="toc-setext-style-headings">Setext-style headings <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#atx-style-headings" id="toc-atx-style-headings">ATX-style headings <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-blank_before_header" id="toc-extension-blank_before_header">Extension: <span class="nowrap"><code>blank_before_header</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-space_in_atx_header" id="toc-extension-space_in_atx_header">Extension: <span class="nowrap"><code>space_in_atx_header</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#heading-identifiers" id="toc-heading-identifiers">Heading identifiers <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-header_attributes" id="toc-extension-header_attributes">Extension: <span class="nowrap"><code>header_attributes</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-implicit_header_references" id="toc-extension-implicit_header_references">Extension: <span class="nowrap"><code>implicit_header_references</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#block-quotations" id="toc-block-quotations">Block quotations <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-blank_before_blockquote" id="toc-extension-blank_before_blockquote">Extension: <span class="nowrap"><code>blank_before_blockquote</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#verbatim-code-blocks" id="toc-verbatim-code-blocks">Verbatim (code) blocks <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#indented-code-blocks" id="toc-indented-code-blocks">Indented code blocks <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#fenced-code-blocks" id="toc-fenced-code-blocks">Fenced code blocks <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-fenced_code_blocks" id="toc-extension-fenced_code_blocks">Extension: <span class="nowrap"><code>fenced_code_blocks</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-backtick_code_blocks" id="toc-extension-backtick_code_blocks">Extension: <span class="nowrap"><code>backtick_code_blocks</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-fenced_code_attributes" id="toc-extension-fenced_code_attributes">Extension: <span class="nowrap"><code>fenced_code_attributes</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#line-blocks" id="toc-line-blocks">Line blocks <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-line_blocks" id="toc-extension-line_blocks">Extension: <span class="nowrap"><code>line_blocks</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#lists" id="toc-lists">Lists <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#bullet-lists" id="toc-bullet-lists">Bullet lists <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#block-content-in-list-items" id="toc-block-content-in-list-items">Block content in list items <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#ordered-lists" id="toc-ordered-lists">Ordered lists <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-fancy_lists" id="toc-extension-fancy_lists">Extension: <span class="nowrap"><code>fancy_lists</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-startnum" id="toc-extension-startnum">Extension: <span class="nowrap"><code>startnum</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-task_lists" id="toc-extension-task_lists">Extension: <span class="nowrap"><code>task_lists</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#definition-lists" id="toc-definition-lists">Definition lists <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-definition_lists" id="toc-extension-definition_lists">Extension: <span class="nowrap"><code>definition_lists</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#numbered-example-lists" id="toc-numbered-example-lists">Numbered example lists <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-example_lists" id="toc-extension-example_lists">Extension: <span class="nowrap"><code>example_lists</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#ending-a-list" id="toc-ending-a-list">Ending a list <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#horizontal-rules" id="toc-horizontal-rules">Horizontal rules <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#tables" id="toc-tables">Tables <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-table_captions" id="toc-extension-table_captions">Extension: <span class="nowrap"><code>table_captions</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-simple_tables" id="toc-extension-simple_tables">Extension: <span class="nowrap"><code>simple_tables</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-multiline_tables" id="toc-extension-multiline_tables">Extension: <span class="nowrap"><code>multiline_tables</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-grid_tables" id="toc-extension-grid_tables">Extension: <span class="nowrap"><code>grid_tables</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-pipe_tables" id="toc-extension-pipe_tables">Extension: <span class="nowrap"><code>pipe_tables</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#metadata-blocks" id="toc-metadata-blocks">Metadata blocks <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-pandoc_title_block" id="toc-extension-pandoc_title_block">Extension: <span class="nowrap"><code>pandoc_title_block</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-yaml_metadata_block" id="toc-extension-yaml_metadata_block">Extension: <span class="nowrap"><code>yaml_metadata_block</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#backslash-escapes" id="toc-backslash-escapes">Backslash escapes <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-all_symbols_escapable" id="toc-extension-all_symbols_escapable">Extension: <span class="nowrap"><code>all_symbols_escapable</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#inline-formatting" id="toc-inline-formatting">Inline formatting <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#emphasis" id="toc-emphasis">Emphasis <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-intraword_underscores" id="toc-extension-intraword_underscores">Extension: <span class="nowrap"><code>intraword_underscores</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#strikeout" id="toc-strikeout">Strikeout <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-strikeout" id="toc-extension-strikeout">Extension: <span class="nowrap"><code>strikeout</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#superscripts-and-subscripts" id="toc-superscripts-and-subscripts">Superscripts and subscripts <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-superscript-subscript" id="toc-extension-superscript-subscript">Extension: <span class="nowrap"><code>superscript</code></span>, <span class="nowrap"><code>subscript</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + commonmark_x
                    + markdown
                    + markdown_mmd
                    + opml

                    disabled by default for:
                    - commonmark
                    - gfm
                    - ipynb
                    - markdown_github
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#verbatim" id="toc-verbatim">Verbatim <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-inline_code_attributes" id="toc-extension-inline_code_attributes">Extension: <span class="nowrap"><code>inline_code_attributes</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#underline" id="toc-underline">Underline <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#small-caps" id="toc-small-caps">Small caps <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#highlighting" id="toc-highlighting">Highlighting <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#math" id="toc-math">Math <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-tex_math_dollars" id="toc-extension-tex_math_dollars">Extension: <span class="nowrap"><code>tex_math_dollars</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#raw-html" id="toc-raw-html">Raw HTML <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-raw_html" id="toc-extension-raw_html">Extension: <span class="nowrap"><code>raw_html</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-markdown_in_html_blocks" id="toc-extension-markdown_in_html_blocks">Extension: <span class="nowrap"><code>markdown_in_html_blocks</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-native_divs" id="toc-extension-native_divs">Extension: <span class="nowrap"><code>native_divs</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-native_spans" id="toc-extension-native_spans">Extension: <span class="nowrap"><code>native_spans</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-raw_tex" id="toc-extension-raw_tex">Extension: <span class="nowrap"><code>raw_tex</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#generic-raw-attribute" id="toc-generic-raw-attribute">Generic raw attribute <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-raw_attribute" id="toc-extension-raw_attribute">Extension: <span class="nowrap"><code>raw_attribute</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#latex-macros" id="toc-latex-macros">LaTeX macros <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-latex_macros" id="toc-extension-latex_macros">Extension: <span class="nowrap"><code>latex_macros</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#links-1" id="toc-links-1">Links <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#automatic-links" id="toc-automatic-links">Automatic links <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#inline-links" id="toc-inline-links">Inline links <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#reference-links" id="toc-reference-links">Reference links <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-shortcut_reference_links" id="toc-extension-shortcut_reference_links">Extension: <span class="nowrap"><code>shortcut_reference_links</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#internal-links" id="toc-internal-links">Internal links <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#images" id="toc-images">Images <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-implicit_figures" id="toc-extension-implicit_figures">Extension: <span class="nowrap"><code>implicit_figures</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-link_attributes" id="toc-extension-link_attributes">Extension: <span class="nowrap"><code>link_attributes</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#divs-and-spans" id="toc-divs-and-spans">Divs and Spans <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-fenced_divs" id="toc-extension-fenced_divs">Extension: <span class="nowrap"><code>fenced_divs</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-bracketed_spans" id="toc-extension-bracketed_spans">Extension: <span class="nowrap"><code>bracketed_spans</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#footnotes" id="toc-footnotes">Footnotes <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-footnotes" id="toc-extension-footnotes">Extension: <span class="nowrap"><code>footnotes</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-inline_notes" id="toc-extension-inline_notes">Extension: <span class="nowrap"><code>inline_notes</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#citation-syntax" id="toc-citation-syntax">Citation syntax <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-citations" id="toc-extension-citations">Extension: <span class="nowrap"><code>citations</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#non-default-extensions" id="toc-non-default-extensions">Non-default extensions <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#extension-rebase_relative_paths" id="toc-extension-rebase_relative_paths">Extension: <span class="nowrap"><code>rebase_relative_paths</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-mark" id="toc-extension-mark">Extension: <span class="nowrap"><code>mark</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-attributes" id="toc-extension-attributes">Extension: <span class="nowrap"><code>attributes</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-old_dashes" id="toc-extension-old_dashes">Extension: <span class="nowrap"><code>old_dashes</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-angle_brackets_escapable" id="toc-extension-angle_brackets_escapable">Extension: <span class="nowrap"><code>angle_brackets_escapable</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-lists_without_preceding_blankline" id="toc-extension-lists_without_preceding_blankline">Extension: <span class="nowrap"><code>lists_without_preceding_blankline</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-four_space_rule" id="toc-extension-four_space_rule">Extension: <span class="nowrap"><code>four_space_rule</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-spaced_reference_links" id="toc-extension-spaced_reference_links">Extension: <span class="nowrap"><code>spaced_reference_links</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-hard_line_breaks" id="toc-extension-hard_line_breaks">Extension: <span class="nowrap"><code>hard_line_breaks</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-ignore_line_breaks" id="toc-extension-ignore_line_breaks">Extension: <span class="nowrap"><code>ignore_line_breaks</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-east_asian_line_breaks" id="toc-extension-east_asian_line_breaks">Extension: <span class="nowrap"><code>east_asian_line_breaks</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-emoji" id="toc-extension-emoji">Extension: <span class="nowrap"><code>emoji</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-tex_math_gfm" id="toc-extension-tex_math_gfm">Extension: <span class="nowrap"><code>tex_math_gfm</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-tex_math_single_backslash" id="toc-extension-tex_math_single_backslash">Extension: <span class="nowrap"><code>tex_math_single_backslash</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-tex_math_double_backslash" id="toc-extension-tex_math_double_backslash">Extension: <span class="nowrap"><code>tex_math_double_backslash</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-markdown_attribute" id="toc-extension-markdown_attribute">Extension: <span class="nowrap"><code>markdown_attribute</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-mmd_title_block" id="toc-extension-mmd_title_block">Extension: <span class="nowrap"><code>mmd_title_block</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-abbreviations" id="toc-extension-abbreviations">Extension: <span class="nowrap"><code>abbreviations</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-alerts" id="toc-extension-alerts">Extension: <span class="nowrap"><code>alerts</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-autolink_bare_uris" id="toc-extension-autolink_bare_uris">Extension: <span class="nowrap"><code>autolink_bare_uris</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-mmd_link_attributes" id="toc-extension-mmd_link_attributes">Extension: <span class="nowrap"><code>mmd_link_attributes</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-mmd_header_identifiers" id="toc-extension-mmd_header_identifiers">Extension: <span class="nowrap"><code>mmd_header_identifiers</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-compact_definition_lists" id="toc-extension-compact_definition_lists">Extension: <span class="nowrap"><code>compact_definition_lists</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-gutenberg" id="toc-extension-gutenberg">Extension: <span class="nowrap"><code>gutenberg</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-sourcepos" id="toc-extension-sourcepos">Extension: <span class="nowrap"><code>sourcepos</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-short_subsuperscripts" id="toc-extension-short_subsuperscripts">Extension: <span class="nowrap"><code>short_subsuperscripts</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#extension-wikilinks_title_after_pipe" id="toc-extension-wikilinks_title_after_pipe">Extension: <span class="nowrap"><code>wikilinks_title_after_pipe</code></span> <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#markdown-variants" id="toc-markdown-variants">Markdown variants <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
-   <a href="#citations" id="toc-citations">Citations <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
    -   <a href="#specifying-bibliographic-data" id="toc-specifying-bibliographic-data">Specifying bibliographic data <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#capitalization-in-titles" id="toc-capitalization-in-titles">Capitalization in titles <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#conference-papers-published-vs.-unpublished" id="toc-conference-papers-published-vs.-unpublished">Conference Papers, Published vs. Unpublished <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#specifying-a-citation-style" id="toc-specifying-a-citation-style">Specifying a citation style <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#citations-in-note-styles" id="toc-citations-in-note-styles">Citations in note styles <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#placement-of-the-bibliography" id="toc-placement-of-the-bibliography">Placement of the bibliography <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#including-uncited-items-in-the-bibliography" id="toc-including-uncited-items-in-the-bibliography">Including uncited items in the bibliography <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#other-relevant-metadata-fields" id="toc-other-relevant-metadata-fields">Other relevant metadata fields <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
-   <a href="#slide-shows" id="toc-slide-shows">Slide shows <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
    -   <a href="#structuring-the-slide-show" id="toc-structuring-the-slide-show">Structuring the slide show <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#powerpoint-layout-choice" id="toc-powerpoint-layout-choice">PowerPoint layout choice <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#incremental-lists" id="toc-incremental-lists">Incremental lists <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#inserting-pauses" id="toc-inserting-pauses">Inserting pauses <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#styling-the-slides" id="toc-styling-the-slides">Styling the slides <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#speaker-notes" id="toc-speaker-notes">Speaker notes <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#columns" id="toc-columns">Columns <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#additional-columns-attributes-in-beamer" id="toc-additional-columns-attributes-in-beamer">Additional columns attributes in beamer <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
    -   <a href="#frame-attributes-in-beamer" id="toc-frame-attributes-in-beamer">Frame attributes in beamer <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#background-in-reveal.js-beamer-and-pptx" id="toc-background-in-reveal.js-beamer-and-pptx">Background in reveal.js, beamer, and pptx <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
        -   <a href="#on-all-slides-beamer-reveal.js-pptx" id="toc-on-all-slides-beamer-reveal.js-pptx">On all slides (beamer, reveal.js, pptx) <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#on-individual-slides-reveal.js-pptx" id="toc-on-individual-slides-reveal.js-pptx">On individual slides (reveal.js, pptx) <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#on-the-title-slide-reveal.js-pptx" id="toc-on-the-title-slide-reveal.js-pptx">On the title slide (reveal.js, pptx) <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
        -   <a href="#example-reveal.js" id="toc-example-reveal.js">Example (reveal.js) <span class="extension-checkbox" title="enabled by default for:
                    + markdown
                    + opml
                    + org
                    + typst

                    disabled by default for:
                    - docx
                    - ipynb
                    - markdown_github
                    - markdown_mmd
                    - markdown_phpextra
                    - markdown_strict
                    - plain
                    " aria-hidden="true">±</span></a>
-   <a href="#epubs" id="toc-epubs">EPUBs <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
    -   <a href="#epub-metadata" id="toc-epub-metadata">EPUB Metadata <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#the-epubtype-attribute" id="toc-the-epubtype-attribute">The <span class="nowrap"><code>epub:type</code></span> attribute <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#linked-media" id="toc-linked-media">Linked media <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#epub-styling" id="toc-epub-styling">EPUB styling <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
-   <a href="#chunked-html" id="toc-chunked-html">Chunked HTML <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
-   <a href="#jupyter-notebooks" id="toc-jupyter-notebooks">Jupyter notebooks <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
-   <a href="#syntax-highlighting" id="toc-syntax-highlighting">Syntax highlighting <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
-   <a href="#custom-styles" id="toc-custom-styles">Custom Styles <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
    -   <a href="#output" id="toc-output">Output <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#input" id="toc-input">Input <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
-   <a href="#custom-readers-and-writers" id="toc-custom-readers-and-writers">Custom readers and writers <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
-   <a href="#reproducible-builds" id="toc-reproducible-builds">Reproducible builds <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
-   <a href="#accessible-pdfs-and-pdf-archiving-standards" id="toc-accessible-pdfs-and-pdf-archiving-standards">Accessible PDFs and PDF archiving standards <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
    -   <a href="#context" id="toc-context">ConTeXt <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#weasyprint" id="toc-weasyprint">WeasyPrint <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#prince-xml" id="toc-prince-xml">Prince XML <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
    -   <a href="#word-processors" id="toc-word-processors">Word Processors <span class="extension-checkbox" title="enabled by default for:
                + markdown
                + opml
                + org
                + typst

                disabled by default for:
                - docx
                - ipynb
                - markdown_github
                - markdown_mmd
                - markdown_phpextra
                - markdown_strict
                - plain
                " aria-hidden="true">±</span></a>
-   <a href="#running-pandoc-as-a-web-server" id="toc-running-pandoc-as-a-web-server">Running pandoc as a web server <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
-   <a href="#running-pandoc-as-a-lua-interpreter" id="toc-running-pandoc-as-a-lua-interpreter">Running pandoc as a Lua interpreter <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
-   <a href="#a-note-on-security" id="toc-a-note-on-security">A note on security <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>
-   <a href="#authors" id="toc-authors">Authors <span class="extension-checkbox" title="enabled by default for:
            + markdown
            + opml
            + org
            + typst

            disabled by default for:
            - docx
            - ipynb
            - markdown_github
            - markdown_mmd
            - markdown_phpextra
            - markdown_strict
            - plain
            " aria-hidden="true">±</span></a>

<span aria-hidden="true">×</span>

<span class="modal-title">Search results</span>

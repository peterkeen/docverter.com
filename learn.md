<!-- --- title: Learn More about Converting Documents with Docverter -->

<div class="hero-unit hero-small">
  <h1>Docverter</h1>
  <p>Use Docverter's REST API to convert your documents, lickety split.</p>
  <p>
    <a class="btn btn-large btn-success" href="https://github.com/docverter/docverter">Get the source &raquo;</a>
  </p>
</div>

<div class="row">
  <div class="span6">
    <h2>What formats does Docverter support?</h2>
    <p>Docverter supports the following input formats:</p>
    <ul>
      <li>Markdown</li>
      <li>Textile</li>
      <li>reStructured Text</li>
      <li>HTML</li>
      <li>Docbook</li>
      <li>LaTeX</li>
    </ul>
    <p>And can convert to these output formats:</p>
    <ul>
      <li>PDF</li>
      <li>HTML</li>
      <li>Microsoft Docx</li>
      <li>OpenOffice ODT</li>
      <li>OpenDocument XML</li>
      <li>EPUB (for iBooks)</li>
      <li>MOBI (for Kindle)</li>
      <li>DocBook</li>
      <li>TexInfo</li>
      <li>Groff</li>
      <li>LaTeX/ConTeXt</li>
      <li>Markdown</li>
      <li>reStructured Text</li>
      <li>AsciiDoc</li>
      <li>MediaWiki</li>
      <li>Emacs Org-Mode</li>
      <li>Textile</li>
    </ul>
  </div>

  <div class="span6">
    <h2>How does Docverter work?</h2>
    <p>When you make an <a href="/api.html">API request</a>, Docverter takes your input documents
      and runs them through <a href="http://johnmacfarlane.net/pandoc/">pandoc</a>, the incomparable document conversion system. Depending on your
      output selection, Docverter may run it through a customized HTML->PDF converter or an ebook
      converter. Docverter then returns the beautifully rendered document to you.</p>
    <br>
    <h2>How long do conversions take?</h2>
    <p>Document conversions typically take less than ten seconds but can occasionally take longer,
      especially if you use the advanced conversion options.</p>
    <br>
    <h2>Can I see some examples?</h2>
    <p>Sure! Here are some examples showing usage of the API. Each example is a zip file containing all of the necessary files and a `convert.sh` shell script.
      <ul>
        <li><a href="/examples/html_to_pdf.zip">HTML to PDF</a> (<a href="/examples/html_to_pdf.pdf">output</a>)</li>
        <li><a href="/examples/html_to_encrypted_pdf.zip">HTML to Encrypted PDF</a> (<a href="/examples/html_to_encrypted_pdf.pdf">output</a>)</li>
        <li><a href="/examples/markdown_to_pdf.zip">Markdown to PDF</a> (<a href="/examples/markdown_to_pdf.pdf">output</a>)</li>
        <li><a href="/examples/markdown_to_epub.zip">Markdown to ePub</a> (<a href="/examples/markdown_to_epub.epub">output</a>)</li>
        <li><a href="/examples/markdown_to_mobi.zip">Markdown to Mobi</a> (<a href="/examples/markdown_to_mobi.mobi">output</a>)</li>
      </ul>
    </p>
  </div>
</div>

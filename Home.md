<div class="hero-unit">
  <h1>Docverter</h1>
  <p>Convert plain text documents written in HTML, Markdown, or LaTeX to PDF, Docx, RTF or ePub with a simple HTTP API.</p>
  <p>
    <a class="btn btn-primary btn-large" href="/learn.html">Learn more &raquo;</a>
    <a class="btn btn-large btn-success" href="https://github.com/docverter/docverter">Get the source &raquo;</a>
  </p>
</div>
<div class="row">
  <div class="span4">
    <h2>Conversion API</h2>
    <p>Docverter has a simple to use HTTP API:
<code><pre>curl http://c.docverter.com/convert \
  -F from=html \
  -F to=pdf \
  -F input_files[]=@%lt(echo hi there)
</code></pre>
    </p>
  </div>
  <div class="span4">
    <h2>Why Docverter?</h2>
    <p>Docverter lets you get going immediately without having to set up your own document conversion tools. Docverter wraps several open source projects to make your documents come out perfect every time.</p>
  </div>
  <div class="span4">
    <h2>Open Source</h2>
    <p>
      Docverter is open source software. Run it on your own hardware or on <a href="http://www.heroku.com">Heroku</a>. Docverter wraps:
<ul>
  <li><a href="http://johnmacfarlane.net/pandoc/">Pandoc</a></li>
  <li><a href="http://calibre-ebook.com/">Calibre</a></li>
  <li><a href="http://code.google.com/p/flying-saucer/">Flying Saucer</a></li>
</ul>
    </p>
  </div>
</div>
<div class="row">
  <div class="span4">
    <div class="button"><a class="btn" href="/api.html">API Documentation &raquo;</a></div>
  </div>
  <div class="span4">
    <div class="button"><a class="btn" href="/learn.html">Frequently Asked Questions &raquo;</a></div>
  </div>
  <div class="span4">
    <div class="button">
      <a class="btn btn-success" href="https://github.com/docverter/docverter">Github &raquo;</a>
      <a class="btn btn-primary" href="https://docverter-donate.herokuapp.com/donate">Donate &raquo;</a>
  </div>
</div>

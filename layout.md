<!doctype html>
<html lang=en>
  <head>
    <meta charset="utf-8">
    <title><%= @page.title == 'Home' ? "" : @page.title + " :: " %>Docverter</title>
    <link href="/bootstrap.min.css" rel="stylesheet">
    <link href="/main.css" rel="stylesheet">
    <!--[if lt IE 9]>
      <script src="http://html5shim.googlecode.com/svn/trunk/html5.js"></script>
    <![endif]-->
    <link rel="apple-touch-icon" href="bugsplat-2.png">

    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="">
    <meta name="author" content="">
    <link rel="shortcut icon" type="image/png" href="/favicon.png" />
    <link rel="icon" type="image/png" href="/favicon.png" />

    <script src="/highlight.pack.js" type="text/javascript"></script>
    <link rel="stylesheet" href="/github.css">

    <script type="text/javascript">
    
      var _gaq = _gaq || [];
      _gaq.push(['_setAccount', 'UA-5663087-9']);
      _gaq.push(['_setDomainName', 'docverter.com']);
      _gaq.push(['_trackPageview']);
    
      (function() {
        var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
        ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
        var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
      })();
    
    </script>

  <script> 
    hljs.tabReplace = '    ';
    hljs.initHighlightingOnLoad();
  </script> 

  </head>
  <body>

    <div class="container">
      <div id="content">
        <%= yield %>
      </div>
      <hr>
      <footer>
        <div class="pull-right">
          <a href="/">Home</a> | <a href="/api.html">API</a> | <a href="/learn.html">FAQ</a> | <a href="https://docverter-donate.herokuapp.com/donate">Donate</a>
        </div>
        <p>&copy; 2012 <a href="http://www.petekeen.com">Pete Keen</a></p>
      </footer>
    </div>
  </body>
</html>

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

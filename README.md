### OpenTrip Documentation

This is a copy of the [opentrip.info](http://opentrip.info) website, which documents the *OpenTrip Core* specification.  The website was originally a wiki, which is now converted to markdown and static html.  It was written in 2008-2009.

* [Home](Home.md) - homepage
* [OpenTrip Core](OpenTrip_Core.md) - specification of the protocol
* [Presentation PDF](OpenTrip_2011mar23.pdf) - *OpenTrip: An Open Protocol for the Interchange of Travel Information Among Rideshare Providers*


#### Notes on transition to new website

* Use [Pandoc](http://pandoc.org) to convert MediaWiki markup to Markdown:
    - pandoc -f markdown_github -t html --template=template.html  < Home.md > Home
    - pandoc -f markdown_github -t html --template=template.html  < OpenTrip_Core.md > OpenTrip_Core

* Apply rewrite rules to redirect links on web server:
    - /wiki/OpenTrip_Core  -->  /OpenTrip_Core
    - /wiki/Home  -->  /Home
    - /  -->  /Home

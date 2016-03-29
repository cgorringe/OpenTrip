
#### Install to website

* Used Pandoc to convert original MediaWiki to Markdown format.

* Next use [Pandoc](http://pandoc.org) to convert Markdown to HTML:
    - pandoc -f markdown_github -t html --template=template.html  < Home.md > Home.html
    - pandoc -f markdown_github -t html --template=template.html  < OpenTrip_Core.md > OpenTrip_Core.html

* Apply rewrite rules to redirect links on web server:
    - /wiki/OpenTrip_Core  -->  /OpenTrip_Core
    - /wiki/Home  -->  /Home
    - /  -->  /Home

#### .htaccess file:

```
Options +MultiViews 
RewriteEngine On
RewriteRule ^$ /Home.html [L]
RewriteCond %{REQUEST_URI} !^/
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^(.*)$ $1.html [L]
```
*( not fully working yet )*

zot_bib_web: Interactive web bibliographies with Zotero
============================================

Example Installations
----------------------------------------

[Lab website at Penn State](http://acs.ist.psu.edu/wp/pub/)

[Personal website](http://david-reitter.com/pub/)


Features
----------------------------------------

This tool generates interactive web bibliographies based on one or
more collections in a Zotero repository.

Collections can be maintained by groups of people, using Zotero's web
interface or their desktop applications.

Zot_bib_web does not depend on any third-party web server.  The
generated bibliographies load quickly because they are stored as static
files along with the rest of your website. 

Setup is very easy.  Just call zot.py with the key of a public Zotero collection.

Bibliographies
-  are grouped by collection,
-  editable in Zotero by one or more users,
-  are interactively searchable,
-  linked to PDF documents or other URLs,
-  have records for BibTex, EndNote and Wikipedia, and
-  can be exported to HTML or pushed to a Wordpress database

The following formats and info can be included:
BibTex, RIS (for EndNote etc.), Wikipedia markup, abstract, PDF.

A readable bibliographic entry (e.g., in APA style) is always shown.
COINS is always included (invisible).

The content generated is static.  This program is meant to
be run periodically.

A tool to push the resulting bibliography to a Wordpress installation
is also provided.

zot_bib_web is available on Github.



Requirements
----------------------------------------
- Python 2.7 or 3
- Pyzotero.  
To install Pyzotero, a library for python:

		sudo pip install pyzotero

	or:

		sudo easy_install pyzotero

- A Zotero collection with your bibliography (as user or as group)


Setup
-----------------------------------------


- ensure zot.py is executable (chmod ug+x zot.py)

- Try it out.  From a unix-like command-line, do this:

		./zot.py --group 160464 DTDTV2EP

   Then view zotero-bib.html in a browser.
   If that looks good, move on to the next steps for configuration.

- In a new file called settings.py, add configuration as documented in
  the file settings.example.py.
  Go to zotero.org to get your API secret key and your user or library
  IDs.  It's easy: see the top of settings.example.py for details.
  If settings.py is set up, you can call zot.py without arguments.

  Alternatively, you can use give the primary settings in arguments to
  the program.


Deployment to a web site
-----------------------------------------

- Upload the site folder or its contents to a public place on your web server.
  By default, /site/... is the assumed URL.

To generate HTML and include it in a website:

-  run zot.py once/on demand, or install as cron job or service on a server
Do not run it more than once a day.  Configure it directly in zot.py,
or in a separate file settings.py to make upgrading simple.

- include the resulting file zotero-bib.html (or as configured) in
  your website as you see fit.  You may also include individual
  collection files, which are also generated.   You can configure
  zot.py to generate a complete HTML document, or just a portion of
  it.  Zot_bib_web generates HTML5 content.

- Style your bibliography using CSS.  An example style
  file is included (see site/ directory).


Bibliography in Zotero
-----------------------------------------

- With Zotero, create a bibliography and note its ID (e.g., from the
  URL in the Zotero web interface).  Example: `MGID90AT`.
  This ID is what you need for the "toplevelfilter" variable in
  settings.py.

- You can and should add sub-collections to your bibliography.

- To define an order for the sub-collections, name them starting with
  a number: "10 Journal Articles".

- To cause zot_bib_web to format a sub-collection in "short" mode, add
  a * at the beginning of the collection name: "05* Selected Works".
  This sub-collection will be shown using titles, journal and years only, which
  can then be expanded.  Journal or conference titles can be kept short.  Specify the
  "journal abbr or "conference title" fields, or a short "note" if
  necessary.

  You may want to copy bibliographic items from other parts of the
  bibliography into this sub-collection.

Here's an example of a bibliography structure:

	My Publications [MGID90AT]
		10* Selected Works
		15 In Preparation / Under Review
		20 Refereed Works by Topic
			Semantics
			Parsing
			Dialogue
			Machine Learning
		30 Theses
		40 Talks (Without Paper)

To see this, set toplevelfilter and catchallcollection to MGID90AT in settings.py.



Wordpress Support
-----------------------------------------

This package can push directly to a Wordpress site.  A separate
program "push.py" is included to do this.

Follow these steps:

1.  Set up zot.py to generate a bibliography you like.
	Call zot.py --full to generate a complete zotero-bib.html file
	for debugging purposes.  Configure settings.py to not generate
	the full HTML code.
2. Install the wpautop-control plugin (or a similar plugin) to make
	sure that WP will not insert paragraph breaks at various places
	in the bibliography.  With this plugin, you will need to add a
	"custom field" to the page created in the  next step (Choose
	"Screen Options" at the top of the page view, enable custom
	fields.  Then find custom fields at the very bottom of the page
	and add a "wpautop" field with value "no".
3.  Create a WP page or a post for the bibliography. Insert
	[zot_bib_web COLLECTION] where you'd like the bibliography
	inserted.  Replace COLLECTION with the ID of the collection.
	(More options: see push.py)
4.  Configure push.py (at the top).  You will need to know a few simple
	details about your WP installation.
5.  Run push.py regularly or on demand.  It will call zot.py
	automatically and then update the page in WP.





How it works
-----------------------------------------

Zot.py produces annotated, beautiful bibliographies for the web from a
Zotero collection.  It is designed for individuals and for research
groups.

This program will load settings.py for its configuration.
It will retrieve all publications for the given user or
group.  If subcollections are present, each subcollection will form a
separate section.  The top-level collection can be ignored (depending
on configuration.)

The output of zot.py consists of HTML: either a full document, or a snippet, as
configured.

Each bibliographic entry is annotated with a bib record and an abstract,
each of which can be revealed by the reader (client-side javascript).



Author
-----------------------------------------
David Reitter, Penn State

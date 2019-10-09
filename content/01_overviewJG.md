<head>
<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1.4.4/jquery.min.js"></script>
<script type="text/javascript">
function toggleDiv(divId) {
   $("#"+divId).toggle();
}
</script>

</head>



# The Jupyter Book Guide: JG

This is a guide to the creation of your own book from content written in
Jupyter Notebooks and in markdown. _**jupyter-book**_ converts book content in
Jupyter Notebook and markdown format to HTML, and then further modifies that HTML (using Jekyll) into a book fit for hosting on the web, either at GitHub, or at any hosting service.

Using additional packages, your book becomes live and interactive, running the Jupyter Notebook content either locally or across the web.  And your mathematical notation is rendered, using .....

_**jupyter-book**_ is a python package that generates book pages in HTML with the `jupyter-book build` command, and then combines those HTML pages into a book with the `jupyter-book serve` command. This second step uses another package, `jekyll`, to build a table of contents column for the book, and a page contents column for each page. The `serve` command also starts a local server on your machine to show your book in your browser.

By pointing your browser to the local address provided by running the `jupyter-book serve` command, you will see your book, served from your local machine.  That `serve` command will continue to run, allowing you to dynamically update book content and see the result immediately in your browser.

Last, to publish your book to the web, you will use a package named `ghp-import`, which you will run in a separate terminal window because your initial terminal is still running `jupyter-book serve`.

Run ` $ pip install ghp-import` once to install the ghp_import package. Though it says "import", it exports your finished HTML pages to GitHub, so GitHub can serve them as a website to the Internet.

Running `$ ghp-import -n -p -f _site` will push your final HTML files to Github to be published. We'll describe how this works, later, after we describe how to set up your GitHub repository.

Your workflow, from this point on, will be:
-  use your editor to change the local content in the /content directory, and save your Jupyter notebooks into your local /content directory
-  save your changes
-  build your book's HTML with `$ jupyter-book build .`,  which is executed in your book's directory
-  look at your book in your browser, using the server running on your local machine with a local address.  See how it looks.
- repeat until satisfied
-  finally, export all the HTML files to GitHub, using `ghp-import`
-  then, look at your book's website in a second browser window, aimed at your web GitHub account, to see what the world will see.

In practice, it looks like this, using the Atom editor as an example, with your book in the directory `JupyterBooks/JBook1`:

```

$ cd JupyterBooks/JBook1   #move into the home directory for your book
$ atom .   #invoke the Atom editor on the directory, which will create a "project",\
 allowing the Atom editor to show all files in the directory

Edit files in /content
Save

$ jupyter-book build .   #This converts the .md and .ipynb files in /content \
into HTML files in _build.  \
These HTML files do not yet have the full page-to-page links needed for a web site. \
Those links will be created by running `make serve`.

$ make serve  #This command does a lot. \
It converts the HTML files in _build into complete web HTML files in _site, using Jekyll to generate the links.  \
And, it fires up a server that runs until stopped, that displays the HTML files in _site. \

In one browser window, go to `http://127.0.0.1:4000/JBook1`
See how your book looks.  Check the links.


If you make a change to a file in /content, open a new Terminal window in the JBook1 directory, and run `$ jupyter-book build .` \
(or `$ make build` , which does the same thing)`.\

Then, just reload the browser to see the result; the server is still running in the other window. \
(On Mac, use `Command-Shift-R`; on PC, `Control-Shift-R`)

Now, move all the files in _site up to the GitHub server, and make sure GitHub knows where to find them.


$ ghp-import -n -p -f _site

In another browser window, go to `https://yourname.github.io/JBook1`

See how it looks. Check the links.

That's it.

```

```
Oh, and when you're used to using the CLI, here's what it looks like after you save your edits:

$ !j
$ !g

```

## Installing _jupyter-book_

You will use a terminal interface: on a Mac, use Terminal or iTerm; on a PC, use .........  We will call it the "command-line interface", or CLI. We use '$' to indicate the beginning of the command line.

The Jupyter-Book CLI allows you to create, build, upgrade, and otherwise control your
Jupyter Book. You can install it via `pip` with the following command that you can execute anywhere:

```
$ pip install jupyter-book
```

Next, create a directory in a convenient place to contain your Jupyter Books. Inside that directory, each book will live in its own directory.

```
$ mkdir JupyterBooks
$ cd JupyterBooks

```

## A quick tour of a Jupyter Book

Jupyter-Book comes with a demo book so that you can see how the content files
are used in the book. We'll begin with a quick tour of these files, as they are
the pieces that you'll modify for your own book.   Build a quick book to get the idea, using the commands below, and then build your own book using the example as a template.

Create a **demo Jupyter Book**. Run the following command:

```
$ jupyter-book create JBook1 --demo
```

A new directory containing the demo book will be created at the path that you've given (in this case, `JupyterBooks/JBook1`).  If JBook1 already exists, it won't let you overwrite it; just rename the JBook1 directory to something else, so `jupyter-book` can create the `JBook1` directory and fill it with the demo files.

Let's take a quick look at some important files in the demo book you created:

```
$ cd JBook1
$ ls
```

Here's what it looks like, showing all the files:

```
JBook1/
├── assets
│   └── custom
│       ├── custom.css
│       └── custom.js
├── _config.yml
├── content
│   ├── features
│   │  ├── features.md
│   │  └── notebooks.ipynb
│   └── LICENSE.md
├── _data
│   └── toc.yml
└── requirements.txt
```

Here's a quick rundown of the files you can modify for yourself, and that
ultimately make up your book.

### Book configuration

All of the configuration for your book is in the following file:

```
mybookname/
├── _config.yml
```
The key fields to define so that the web links are built correctly are `url` and `baseurl`.
`url` should be the URL for your GitHub account, and look like `url: yourname.github.io`. baseurl is the name of your remote repository, and should look like `/JBook1` .

You can define metadata for your book (such as its title), add
a book logo, turn on different "interactive" buttons (such as a
Binder button for pages built from a Jupyter Notebook), and more.

### Book content

Your book's content can be found in the `content/` folder. Some content
files for the demo book are shown below:

```
JBook1/
├── content
    └── features
       ├── features.md
       └── notebooks.ipynb
```

Note that the content files are either **Jupyter Notebooks** or **Markdown**
files. These are the files that define "pages" in your book.

You can store these files in whatever collection of folders you'd like, note that
the *structure* of your book when it is built will depend solely on the order of
items in your `_data/toc.yml` file (see below section)

### Table of Contents

Jupyter Book uses your Table of Contents to define the structure of your book.
For example, your chapters, sub-chapters, etc.

The Table of Contents lives at this location:

```
JBook1/
├── _data
    └── toc.yml
```

This is a YAML file with a collection of pages, each one linking to a
file in your `content/` folder. Here's an example of a few pages defined in `toc.yml`.

```yaml
- title: Features and customization
  url: /features/features
  not_numbered: true
  expand_sections: true
  sections:
  - title: Markdown files
    url: /features/markdown
    not_numbered: true
  - title: Jupyter notebooks
    url: /features/notebooks
    not_numbered: true
```

The top-most level of your TOC file are **book chapters**. Above, this is the
"Features and customization" page. Each chapter can have
several sections (defined in `sections:`) and each section can have several sub-sections
(which would be define with a deeper level of `sections:`). In addition, you can
use a few extra YAML values to control the behavior of Jupyter-Book (for example,
`not_numbered: true` will prevent Jupyter Book from numbering the pages in that chapter).

Each item in the YAML file points to a single content file. The links
should be **relative to the `/content/` folder and with no extension.**

For example, in the example above there is a file in
`mybookname/content/features/notebooks.ipynb`. The TOC entry that points to
this file is here:

```yaml
    - title: Jupyter notebooks
        url: /features/notebooks
```

### A license for your content

When you share content online, it's a good idea to add a license so that others know
what rights you retain to the work. This can make your book more sharable and (re)usable.

The license for a Jupyter Book lives in this location:

```
mybookname/
├── content
    └── LICENSE.md
```

When you create a new book, if you don't specify a license, then `jupyter-book` will by default
add a [Creative Commons Attribution-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/)
(CC BY-SA 4.0) license to your book. CC BY-SA requires attribution of
your work, and also requires that any derivations someone creates are released
under a license *at least as permissive* as CC BY-SA.

If you'd like to choose a different license, you can add whatever text you like to the file
in `/content/LICENSE.md`. We commend checking out the [Creative Commons licenses page](https://creativecommons.org/licenses)
for several options for licenses that may suit your needs.

### Book code requirements files

Since your Jupyter Book likely has computational material specified in Jupyter
Notebooks, you should specify the packages needed to run your Jupyter Book.
In this case, we use a `requirements.txt` file:

```
mybookname/
└── requirements.txt
```

The demo book uses `requirements.txt` because it has Python code, but you can
include any other files that you'd like to.

### Book bibliography for citations

If you'd like to build a bibliography for your book, you can do so by including
the following file:

```
mybookname/
├── _bibliography
    └── references.bib
```

This BiBTex file can be used along with the `jekyll-scholar` extension. For more information
on how to use citations in Jupyter Book, see [Citations with Jupyter Book](../features/citations)

### Custom Javascript and CSS

These are the files in this location:

```
├── assets
    └── custom
        ├── custom.css
        └── custom.js
```

Jupyter Book lets you supply your own CSS and Javascript that will be
built into your book. Feel free to edit these files with whatever you like.

## Next section

Now that you're familiar with the Jupyter Book structure, head to the next section
to learn how to create your own!

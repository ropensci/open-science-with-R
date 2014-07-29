Outline

* <a href="#intdb">What is an "internet database"?</a>
* <a href="#online">The power of online data</a>
* <a href="#find">Finding data online</a>
* <a href="#move">When to move from the browser to R when using online data.</a>
* <a href="#diss">Dissappearing data.</a>
* <a href="#change">Database content and interfaces can change.</a>
* <a href="#sens">Sensitive data.</a>
* <a href="#list">Internet databases - a non-exhuastive list.</a>
* <a href="#sustain">Database sustainability.</a>
* <a href="#apis">Think APIs first</a>
* <a href="#diy">Roll your own database</a>
* <a href="#def">Definitions</a>

## <a href="#intdb" name="intdb">#</a> What is an "internet database"?

First, some defintions. A _database_ is typically one or more tables of data, often connected to one another through common fields to allow fast queries. Databases are often defined as structured sets of data. This is in contrast to the most common bucket of data, the _spreadsheet_ or _dataset_, most commonly as an Excel file (`.xls`, `.xlsx`), or as a `.csv` file. These files hold data, but are not databases themselves. Putting a number of single spreadsheets together in e.g. PostgreSQL creates a database. Databases come in two flavors: SQL and No-SQL. SQL databases are collections of row-column oriented data (i.e. tables) connected to one another by similar columns. These include [MySQL](#), [PostgreSQL](#), [SQLite](#), [Microsoft SQL Server](#), [Oracle SQL](#), and more. NoSQL databases are a somewhat heterogeneous mix of databases that somtimes do not have a row-colmn table like structure to the data, and are often schema free. Some NoSQL databases converse entirely in [JSON (JavaScript Object Notation)](#), and store _documents_ instead of tables (e.g., [MongoDB](#), [CouchDB](#)), while others are more simple _key-value_ stores, that use keys (e.g. `myblobofdata`) to point to a blob of data (e.g., [Redis](#), [LevelDB](#), [Riak](#)). And others are graph databases like [Neo4J](#).

A database holds data in a structured way. This structure can vary quite a bit (see above), but the salient point is that any database you have locally can also go online with a few bells and whistles to allow a user to interact with the database. A database available over the internet is stored on a computer somewhere, just as when it is stored on your own - except a few tricks allow you to query that database _over the wire_.

## <a href="#online" name="online">#</a> The power of online data

For some science fields, perhaps all their raw data to answer questions comes from the internet, whereas it used to come from books pre 21st century. For them, this is a non-issue. However, there are many fields - ecology, geology, archeology, etc. - that collect a lot of new data literally in the field, i.e., outdoors. This is essential work. Much of this new data collected is in the form of physical things stored in lab notebooks, spreadhseets, museums, and libraries. Metadata and data describing this material is moving online at a fast pace. This is a part of the _big data_ revolution.

Many science questions or at least parts of questions can likely utilize data online, whether it be metadata or full text of published articles, climate data, biological specimen data, or molecular sequence data. Some fields, like macroecology, almost entirely use data collected by other people to ask questions at large spatial scales like _How general is the relationship between area sampled and biological diversity (i.e., number of species)?_. This question fundamentally requires a lot of data.

As there are some scientists that may use data only from the internet for their work, some scientists may still benefit from _some_ online data use. One example is taxonomic data for biologists. Whether a biologist is studying a single species or a community of 1000 species, they often do not know the current state of the higher taxonomy, that is, the groupings above the species level that organize biological diversity. There are a large number of online databases for taxonomic information, many of which are avaiallable programatically through the `taxize` R package (Python version coming soon). Even if a biologist collects all her data in the field or in the lab, she still is likely to use the internet at some point in her research process to get information on her study species. For example, she may wonder "What is known about what my study species eats?" - for most people this will lead to a Google search, perhaps leading to an online database of information on the species. As the list of species for which one wants information grows longer, the repetitiveness of the task becomes more apparent, and the utility of a programatic approach becomes more appealing.

There is another reason to use online data - why repeat what others have already done? As Google so aptly states on the landing page of Google Scholar, "Stand on the shoulders of giants". Whatever your question, someone else is likely to have learned something about it. This is the way we can make broad claims about the affects of a particular treatment on a disease, by researchers collating studies on a topic, and drawing inferences from all of them together, instead of going out and collecting new data.

XXXX more...

<!-- not super satisfying yet, need some better reasoning here -->

## <a href="#find" name="find">#</a> Finding data online

Where to find data online will be highly dependent on your field. Regardless of field, perhaps all naive data searches begin by "Googling" keywords for the data of interest.

## <a href="#move" name="move">#</a> When to move from the browser to R when using online data

The typical scientist is likely to start in the browser to find the data they are after, then continue to download data from the website, then perhaps open in Excel to view, then finally import to R. The data discovery process likely will always start in a browser, but the data collection part becomes much more efficient and easier in R with each additional form field to fill out on a web form.

Let's consider two scenarios.

In the first scenario, our scientist Jane Doe wants to understand X question. She needs to get species occurrence data for 3 species. She hears about GBIF through a friend, and decides to check it out. Upon landing on the GBIF website, she figures out what she wants after navigating through a few pages, enters a search term for species A, and downloads the data. She repeats the process for the remaining two species.

In the second scenario, Jon Doe wants to understand X question, but is interested in modeling data for 500 species. He heard about GBIF through Jane of course, and decided to visit the site to get a sense for what data were available. He decided that data were appropriate, and started to download data for each species. After about 6 species, Jon had a nagging feeling that there must be a better way. After all, why repeat what you can automate. With a progamming language, Jon can repeat the download task for all 500 species with the same set of code, and pass in his species list to the code that gets the data.

## <a href="#diss" name="diss">#</a> Disappearing data

Despite the benefits of online data, there are potential costs. There are a variety of things to consider with online data: whether the data exists any longer, the speed at which the server/database online provide the data, and your internet connection speed. Any of these things can influence the speed at which you get data back.

See below for more discussion of caching.

## <a href="#change" name="change">#</a> Database content and interfaces can change

Although interacting with online databases can be a great advantage to your science, there are a few gotchas. These fall in to two categories: 1) interface changes; and 2) content changes.

### <a href="#def" name="def">#</a> Interface

Every online database needs to provide some way for the user to inspect the database. Let's call this the "database interface". The database interface, like any other piece of software, can change through time as the software developer's decide on a better interface. The interface is not only what you see in a browser but how you programatically talk to the database. What you see on a website is not so important as how you talk to a database programatically. That is, if you are accessing an online database with a script written in R or Python, if the programmatic way of interfacing to the database changes, your workflow can break. This is a good thing to be aware of when you access online databases - changes are beyond your control. If you are using an online database that many others use, it is likely that someone has done the work of creating a set of functions, or a library, that you can use to access the database. Ideally, this library updates quickly when there are changes to the interface of the online database, which will ideally not break your code. Of course, be aware that code in early development will update frequently - but more mature code should be more stable.

### <a href="#def" name="def">#</a> Content

Most online databases do not stagnate, but grow as more data is added to them. Just like Wikipedia grows though time as new pages are added and new editors edit a page, online databases incorporate new data as they become available. Addtional data is additional knowledge. However, as content changes, even a single additional row in the database, your workflow changes. That is, if your code grabs data from an online database on May 5, 2012, then you query the database again one month later on June 5, 2012, you may get a different response from the database. This is of course not surprising.

However, this is a major concern for reproducbility. Ther are a number of options one it comes to scientific reproducibility in the context of changing online database content:

* **Cache your data** Ideally this is the raw data that the online database provides, which your scripts then process to pass down through your workflow. However, this could be somewhat processed data to make it more consumable by others. Ideally you would provide this data along with your published paper, or as a stand alone dataset.
* **Data provider stores a cache of your data request** Databases can be versioned so that each change is stored, sort of like you can look back through versions of each file on Dropbox. This is the same with any database online. However, in practice online database rarely allow you to cache your data request, and if they do it's for maybe 24 hours or a few weeks. For science, ideally we would want indefinite caching. However, the infrastructure just isn't there right now to do this.

## <a href="#sens" name="sens">#</a> Sensitive data

Some data is sensitive. And of course this sensitive data should be taken seriously. A way to deal with this on the provider side is to use authentication. Of course this won't guarantee security, but can go along way. The many unpredictable findings that could come from exposint data to the public likely outweighs the costs of data getting out, in most cases. Sensitive personal data is correctly tightly held, and is an exception to the rule.

## <a href="#list" name="list">#</a> Scientific internet databases - a non-exhuastive list

What follows is a short list of online databases of at least some scientific use. XXXX <!-- some discussion of them could be inormative, like the pros and cons of different ones, what are aspects of good online databases -->

| Database | Type of data| Field/Topic |
|----------| ------------|-------------|
| GBIF	   | Species occurrence data | Biology |
| Figshare| Datasets | All |
| PLOS article search| Article full text | Text-mining |
| PLOS article-level metrics | Article use metrics | Bibliometrics|
| Biodiversity Heritage Library | Full text | Biology/text-mining |
| GenBank | Genetic sequence data  | Biology |
| NOAA | Climate data, paleobiology datasets | Biology/Geology |
| etc.|  etc.  | etc.  |

## <a href="#sustain" name="sustain">#</a> Database sustainability

Serving up a database on the internet may have been hard 20 years ago, but today it is quite trivial in terms of costs and ease of setup (see next section). Often the limiting factor is cost, as hosting does add up over time, and as increased server sizes are needed. There are various solutions to the funing problem. First, if you are at a university they may give you free server space for the database. Second, small grants of a few thousand dollars could probably pay for a few years of hosting costs. Third, integrating your data into a larger data provider is a good route, and your data may be more likely to be seen. For example, if you have species occurrence record data, instead of providing your data yourself, you can submit the data to the Global Biodiversity Information Facility (GBIF), a global warehouse for these kind of data.

## <a href="#apis" name="apis">#</a> Think APIs first

It is probably natural, given a set of data, to think first about the GUI, or the Graphical User Interface - rather, how users will interact with the data. However, to have a wider impact, if we think in terms of a web of data sets that are programatically writable and readable, we can create a much better data ecosystem. In other words, a great way to limit the use of your data is to completely ignore programmatic access, and only provide browser based methods of interacting with the data. Unfortunately, far too many scientific and government databases completely ignore programmatic access.

## <a href="#diy" name="diy">#</a> Roll your own database

If you already have a database you don't have to do a lot more work to make it available online. You only need to do two additional things: i) make your database available online, and ii) make a web frontend for people to interact with your database. There are multitude of frameworks out there to easily hook up a server to your database (e.g., Heroku, AWS EC2, Rackspace, etc.). There are many frameworks that are easy to use to slap on a web frontend, e.g,. Bootstrap and Foundation. See the _Sharing Data_ chapter for more information on sharing data.

If you have some data, but you don't already have them in a database, you may want to start by learning databases with [SQLite](#), which is a super light weight SQL database. When it comes time to have your database serve data to the public on the web, you likely will want to change to something like PostgreSQL or MySQL.

If the above is too advanced, just getting data out on the web is a great start.


## <a href="#def" name="def">#</a> Some definitions

* spreadsheet
* dataset
* database
* API
*

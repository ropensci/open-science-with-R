Outline

-   What is an "internet database"?
-   Why use online data.
-   Finding data online.
-   When to move from the browser to R when using online data.
-   Database content and interfaces can change.
-   Internet databases - a non-exhuastive list.
-   Roll your own database.

What is an "internet database"?
-------------------------------

A database holds data in a structured way. This structure can vary quite
a bit, but the salient point is that any database you have locally can
also go online with a few bells and whistles to allow a user to interact
with the database.

Why use online data
-------------------

For some science fields, perhaps all their raw data to answer questions
comes from the internet, whereas it used to come from books pre 21st
century. For them, this is a non-issue. However, there are many fields -
ecology, geology, archeology, etc. - that collect a lot of new data
literally in the field, i.e., outdoors. This is essential work. Much of
this new data collected is in the form of physical things stored in lab
notebooks, spreadhseets, museums, and libraries. Metadata and data
describing this material is moving online at a fast pace. This is a part
of the *big data* revolution.

Many science questions or at least parts of questions can likely utilize
data online, whether it be full text of published articles, climate
data, biological specimen data, or molecular sequence data.

Finding data online
-------------------

Where to find data online will be highly dependent on your field.
Regardless of field, perhaps all naive data searches begin by "Googling"
keywords for the data of interest.

When to move from the browser to R when using online data
---------------------------------------------------------

The typical scientist is likely to start in the browser to find the data
they are after, then continue to download data from the website, then
perhaps open in Excel to view, then finally import to R. The data
discovery process likely will always start in a browser, but the data
collection part becomes much more efficient and easier in R with each
additional form field to fill out on a web form.

Let's consider two scenarios.

In the first scenario, our scientist Jane Doe wants to understand X
question. She needs to get species occurrence data for 3 species. She
hears about GBIF through a friend, and decides to check it out. Upon
landing on the GBIF website, she figures out what she wants after
navigating through a few pages, enters a search term for species A, and
downloads the data. She repeats the process for the remaining two
species.

In the second scenario, Jon Doe wants to understand X question, but is
interested in modeling data for 500 species. He heard about GBIF through
Jane of course, and decided to visit the site to get a sense for what
data were available. He decided that data were appropriate, and started
to download data for each species. After about 6 species, Jon had a
nagging feeling that there must be a better way. After all, why repeat
what you can automate. With a progamming language, Jon can repeat the
download task for all 500 species with the same set of code, and pass in
his species list to the code that gets the data.

Database content and interfaces can change
------------------------------------------

Although interacting with online databases can be a great advantage to
your science, there are a few gotchas. These fall in to two categories:
1) interface changes; and 2) content changes.

### Interface

Every online database needs to provide some way for the user to inspect
the database. Let's call this the "database interface". The database
interface, like any other piece of software, can change through time as
the software developer's decide on a better interface. The interface is
not only what you see in a browser but how you programatically talk to
the database. What you see on a website is not so important as how you
talk to a database programatically. That is, if you are accessing an
online database with a script written in R or Python, if the
programmatic way of interfacing to the database changes, your workflow
can break. This is a good thing to be aware of when you access online
databases - changes are beyond your control. If you are using an online
database that many others use, it is likely that someone has done the
work of creating a set of functions, or a library, that you can use to
access the database. Ideally, this library updates quickly when there
are changes to the interface of the online database, which will ideally
not break your code. Of course, be aware that code in early development
will update frequently - but more mature code should be more stable.

### Content

Most online databases do not stagnate, but grow as more data is added to
them. Just like Wikipedia grows though time as new pages are added and
new editors edit a page, online databases incorporate new data as they
become available. Addtional data is additional knowledge. However, as
content changes, even a single additional row in the database, your
workflow changes. That is, if your code grabs data from an online
database on May 5, 2012, then you query the database again one month
later on June 5, 2012, you may get a different response from the
database. This is of course not surprising.

However, this is a major concern for reproducbility. Ther are a number
of options one it comes to scientific reproducibility in the context of
changing online database content:

-   **Cache your data** Ideally this is the raw data that the online
    database provides, which your scripts then process to pass down
    through your workflow. However, this could be somewhat processed
    data to make it more consumable by others. Ideally you would provide
    this data along with your published paper, or as a stand alone
    dataset.
-   **Data provider stores a cache of your data request** Databases can
    be versioned so that each change is stored, sort of like you can
    look back through versions of each file on Dropbox. This is the same
    with any database online. However, in practice online database
    rarely allow you to cache your data request, and if they do it's for
    maybe 24 hours or a few weeks. For science, ideally we would want
    indefinite caching. However, the infrastructure just isn't there
    right now to do this.

Scientific internet databases - a non-exhuastive list
-----------------------------------------------------

What follows is a short list of online databases of at least some
scientific use. XXXX
<!-- some discussion of them could be inormative, like the pros and cons of different ones, what are aspects of good online databases -->

  Database                        Type of data                          Field/Topic
  ------------------------------- ------------------------------------- ---------------------
  GBIF                            Species occurrence data               Biology
  Figshare                        Datasets                              All
  PLOS article search             Article full text                     Text-mining
  PLOS article-level metrics      Article use metrics                   Bibliometrics
  Biodiversity Heritage Library   Full text                             Biology/text-mining
  GenBank                         Genetic sequence data                 Biology
  NOAA                            Climate data, paleobiology datasets   Biology/Geology
  etc.                            etc.                                  etc.

TL;DR: Roll your own database
-----------------------------

If you already have a database you don't have to do a lot more work to
make it available online. You only need to do two additional things: i)
make your database available online, and ii) make a web frontend for
people to interact with your database. There are multitude of frameworks
out there to easily hook up a server to your database (e.g., Heroku, AWS
EC2, Rackspace, etc.). There are many frameworks that are easy to use to
slap on a web frontend, e.g,. Bootstrap and Foundation. See the *Sharing
Data* chapter for more information on sharing data.

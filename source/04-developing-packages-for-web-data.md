*Outline*

-   Why an R package?
-   The basics of Developing R packages <!-- summarize hadley's tips -->
-   R package conventions and best practices
-   Package development philosophy <!-- following hadley's -->
-   Writing functions to interact with web data
    -   APIs
    -   FTP
    -   Direct file downloads
    -   Scraping XML/HTML
    -   Authentication
    -   HTTP error codes
    -   Exposing curl options for advanced users
-   Documenting packages
-   Documenting functions
-   Testing <!-- testthat -->
-   Continuous integration <!-- travis -->
-   Publishing to CRAN <!-- devtools::release -->
-   Quick reference
-   Further reading

Why an R package?
-----------------

Going from less to more complicated: a few lines of code to accomplish a
task, a function to generalize that task, an R package to accomplish a
similar set of tasks. An R package can be complicated to make, but can
have great payoffs even if the package is only for your own use. R
packages have their own namespaces so you can embed function names that
may clash with others within the package name like
`packagename::functionname`. In addition, you can easily add
documentation with parameter explanation and examples for ease of use.
There is an easy to use framework for incorporating tests as well (see
*Testing* section below), essential for creating robust software. The
`devtools` package was created to make the package creating process easy
- we'll go over `devtools` more below.

The basics of developing R packages
-----------------------------------

Hadley Wickham has a nice overview of general R package creation in his
book [**Advanced R programming**](http://adv-r.had.co.nz/). A general
workflow for creating an R package is as follows (let's say the package
name is *foobar* for this example):

``` {.coffee}
library('devtools')
create('foobar')
```

Optionally, you can set the path to the package in the call to
`create()`, which creates a package skeleton with the basic files you
need to make a package:

    /foobar
      /DESCRIPTION
      /man
      /R/foobar-package.r

For each function or set of similar functions create a file in the `R/`
directory.

R package conventions and best practices
----------------------------------------

Package development philosophy
------------------------------

Writing functions to interact with web data
-------------------------------------------

Documenting packages
--------------------

Documenting functions
---------------------

Testing
-------

Testing has become quite easy in R packages with the recent arrival of
the [`testthat`
package](http://cran.r-project.org/web/packages/testthat/index.html).

Continuous integration
----------------------

Continuous integration is a way to integrate your R package with a
framework for building, testing, and running any examples from your
package. The easiest way to do this is using
[Travis-CI](https://travis-ci.org/). Travis-CI builds your package, and
runs any tests. etc. that you describe in a `.travis.yml` file. There is
an ongoing project lead by Google's [Craig
Citro](https://github.com/craigcitro/) to better support R within
Travis-CI. The following is an example `.travis.yml` file:

    language: c
    script: ./travis-tool.sh run_tests
    before_script:
    - curl -OL http://raw.github.com/craigcitro/r-travis/master/scripts/travis-tool.sh
    - chmod 755 ./travis-tool.sh
    - sudo apt-get update
    - sudo apt-get install gdal-bin libgdal1 libgdal1-dev netcdf-bin libproj-dev
    - ./travis-tool.sh bootstrap
    - ./travis-tool.sh install_deps
    - ./travis-tool.sh github_package assertthat

Putting this `.travis.yml` file in the root of your R package will run a
build on Travis-CI on each of your commits to your Github repository.
Remember to add `.travis.yml` to your `.Rbuildignore` file in your R
package so that the file is ignored on R builds.

Publishing to CRAN
------------------

Quick reference
---------------

-   Create package (from `devtools` package): `create("pkg_name")`
-   Run tests in package (from `testthat` package):
    `test_package("pkg_name")` or `check("pkg_name")` will run tests as
    part of `R CMD CHECK` process
-   Publish package to CRAN (from `devtools` package):
    `release("pkg_name")`
-   

Further reading
---------------

-   Hadley Wickham has a book in progress called Advanced R programming,
    which has a number of sections on developing R packages. Find it at
    <http://adv-r.had.co.nz/>.
-   Hadley Wickham has a set of best practices for developing R packages
    to interact with web APIs within the `httr` package at
    <https://github.com/hadley/httr/blob/master/vignettes/api-packages.Rmd>.
-   etc...

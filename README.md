
<!-- README.md is generated from README.Rmd. Please edit that file -->
rrtools: Tools for Writing Reproducible Research in R
=====================================================

[![Travis-CI Build Status](https://travis-ci.org/benmarwick/rrtools.svg?branch=master)](https://travis-ci.org/benmarwick/rrtools) [![Circle-CI Build Status](https://circleci.com/gh/benmarwick/rrtools.svg?style=shield&circle-token=:circle-token)](https://circleci.com/gh/benmarwick/rrtools)

The goal of rrtools is to provide instructions, templates, and functions for making a basic compendium suitable for doing reproducible research with [R](https://www.r-project.org). This package documents the key steps and provides convenient functions for quickly creating a new research compendium. The approach is based generally on Kitzes et al. (2017), and more specifically on Marwick (2017) and Wickham's (2017) work using the R package structure as the basis for a research compendium.

rrtools provides a template for doing scholarly writing in a literate programming environment using [R Markdown](http://rmarkdown.rstudio.com) and [bookdown](https://bookdown.org/home/about.html). It also allows for isolation of your computational environment using [Docker](https://www.docker.com/what-docker), package versioning using [MRAN](https://mran.microsoft.com/documents/rro/reproducibility/), and continuous integration using [Travis](https://docs.travis-ci.com/user/for-beginners). It makes a convenient starting point for writing a journal article, report, or thesis.

The functions in rrtools allow you to use R to easily follow the best practices outlined in several major scholarly publications on reproducible research. In addition to those cited above, Wilson et al. (2017), Piccolo & Frampton (2016), Stodden & Miguez (2014) and rOpenSci (2017a, b) are important sources that have influenced our approach to this package. Please read those before using this package.

This project was developed during the 2017 Summer School on Reproducible Research in Landscape Archaeology at the Freie Universität Berlin (17-21 July), funded and jointly organized by [Exc264 Topoi](https://www.topoi.org/), [CRC1266](http://www.sfb1266.uni-kiel.de/en), and [ISAAKiel](https://isaakiel.github.io/). Special thanks to [Sophie C. Schmidt](https://github.com/SCSchmidt) for help. The convenience functions in this package are derived from similar functions in Hadley Wickham's [`devtools`](https://github.com/hadley/devtools) package.

Installation
------------

You can install rrtools from github with:

``` r
# install.packages("devtools")
devtools::install_github("benmarwick/rrtools")
```

How to use
----------

To create a reproducible research compendium using the rrtools approach, follow these steps. We use [RStudio](https://www.rstudio.com/products/rstudio/#Desktop), and recommend it, but is not required for these steps to work. We recommend copy-pasting these directly into your console, and editing the options before running. We don't recommend saving these lines in a script in your project: they are meant to be once-off setup functions.

#### 1. `rrtools::use_compendium("pkgname")`

-   this uses `devtools::create()` to create a basic R package with the name `pkgname` (you should use a different one), and then, if you're using RStudio, opens the project. If you're not using RStudio, it sets the working directory to the `pkgname` directory.
-   we need to:
    -   choose a location for the compendium package. We recommend two ways to do this. First, you can specify the full path directly, (e.g., `rrtools::use_compendium("C:/Users/bmarwick/Desktop/pkgname")`). Alternatively, you can set the working directory in RStudio using the drop-down menu: `Session` -&gt; `Set Working Directory` and then run `rrtools::use_compendium("pkgname")`.
    -   edit the `DESCRIPTION` file (located in your `pkgname` directory) to include accurate metadata
    -   periodically update the `Imports:` section of the `DESCRIPTION` file with the names of packages used in the code we write in the Rmd document(s) (e.g., `devtools::use_package("dplyr", "imports")`)

#### 2. `devtools::use_mit_license(copyright_holder = "My Name")`

-   this adds a reference to the MIT license in the [DESCRIPTION](DESCRIPTION) file and generates a [LICENSE](LICENSE) file listing the name provided as the copyright holder
-   to use a different license, replace this line with `devtools::use_gpl3_license(copyright_holder = "My Name")`, or follow the instructions for other licenses

#### 3. `devtools::use_github(".", auth_token = "xxxx", protocol = "https", private = FALSE)`

-   if you are connected to the internet, this initializes a local git repository, connects to [GitHub](https://github.com), and creates a remote repository
-   if you are not connected to the internet, use `devtools::use_git(".")` to initialise a local git repository for your project. Reopen your project in RStudio to see the git buttons on the toolbar.
-   we need to:
    -   install and configure git *before* running this line. See [Happy Git With R](http://happygitwithr.com) for details.
    -   get a [personal access token](https://github.com/settings/tokens), and replace "xxxx" with that token. When you do so (click "Generate new token"), make sure the "repo" scope is included by checking the "repo" box. Don't save this token in your project, keep it elsewhere.
-   we have found that this function can be a little unreliable in RStudio, sometimes giving an error and not fully enabling git in RStudio. To work around this:
    -   in the shell, run `git remote set-url origin https://github.com/username/pkgname.git`
    -   restart RStudio, then we can commit, push, pull etc. as usual

#### 4. `rrtools::use_readme_rmd()`

-   this generates [README.Rmd](README.Rmd) and renders it to [README.md](README.md), ready to display on GitHub. It contains:
    -   a template citation to show others how to cite your project. Edit this to include the correct title and [DOI](https://doi.org).
    -   license information for the text, figures, code and data in your compendium
-   this also adds two other markdown files: a code of conduct for users [CONDUCT.md](CONDUCT.md), and basic instructions for people who want to contribute to your project [CONTRIBUTING.md](CONTRIBUTING.md), including for first-timers to git and GitHub.
-   render this document after each change to refresh [README.md](README.md), which is the file that GitHub displays on the repository home page

#### 5. `rrtools::use_analysis()`

-   this has three `location` options: create a top-level `analysis/` directory, create an `inst/` directory (so that all the sub-directories are available after the package is installed), or create a `vingettes/` directory (and automatically update the `DESCRIPTION`). The default is a top-level `analysis/`.
-   for each option, the contents of the sub-directories are the same, with the following (using the default `analysis/` for example):

<!-- -->

    analysis/
    |
    ├── paper/
    │   ├── paper.Rmd         # this is the main document to edit
    │   ├── references.bib    # this contains the reference list information
    │   └── journal-of-archaeological-science.csl
    |                         # this sets the style of citations & reference list
    ├── figures/
    |
    ├── data/
    │   ├── raw_data/       # data obtained from elswhere
    │   └── derived_data/   # data generated during the analysis
    |
    └──  templates
        ├── template.docx  # used to style the output of the paper.Rmd
        └── template.Rmd

-   the `paper.Rmd` is ready to write in and render with bookdown. It includes:
    -   a YAML header that identifies the `references.bib` file and the supplied `csl` file (to style the reference list)
    -   a colophon that adds some git commit details to the end of the document. This means that the output file (HTML/PDF/Word) is always traceable to a specific state of the code.
-   the `references.bib` file has just one item to demonstrate the format. It is ready to insert more reference details.
-   you can replace the supplied `csl` file with a different citation style from <https://github.com/citation-style-language/>
-   we recommend using the [citr addin](https://github.com/crsh/citr) and [Zotero](https://www.zotero.org/) to efficiently insert citations while writing in an Rmd file
-   remember that the `Imports:` field in the `DESCRIPTION` file must include the names of all packages used in analysis documents (e.g. `paper.Rmd`)

#### 6. `rrtools::use_dockerfile()`

-   this creates a basic Dockerfile using [`rocker/verse`](https://github.com/rocker-org/rocker) as the base image
-   the version of R in your rocker container will match the version used when you run this function (e.g., `rocker/verse:3.4.0`)
-   [`rocker/verse`](https://github.com/rocker-org/rocker) includes R, the [tidyverse](http://tidyverse.org/), RStudio, pandoc and LaTeX, so compendium build times are very fast on travis
-   we need to:
    -   edit the Dockerfile to add linux dependencies (for R packages that require additional libraries outside of R). You can find out what these are by browsing the [DESCRIPTION](DESCRIPTION) files of the other packages you're using, and looking in the SystemRequirements field for each package. If you are getting build errors on travis, check the logs. Often, the error messages will include the names of missing libraries.
    -   modify which Rmd files are rendered when the container is made
    -   have a public GitHub repo to use the Dockerfile that this function generates. It is possible to keep the repository private and run a local Docker container with minor modifications to the Dockerfile that this funciton generates. Or we can use `rrtools::use_circleci()` to build our Docker container privately at <https://circleci.com>, from a private GitHub repo.
-   If we want to use Travis on our project, we need to make an account at <https://hub.docker.com/> to receive our Docker container after a successful build on travis

#### 7. `rrtools::use_travis()`

-   this creates a minimal `.travis.yml` file. By default it configures travis to build our Docker container from our Dockerfile, and build, install and run our custom package in this container. By specifying `docker = FALSE` in this function, the travis file will not use Docker in travis, but run R directly on the travis infrastructure. We recommend using Docker because it offers greater computational isolation and saves a substantial amount of time during the travis build because the base image contains many pre-compiled packages.
-   we need to:
    -   go to <https://travis-ci.org/> to connect to our repo
    -   add environment variables to enable push of the Docker container to the Docker Hub
    -   make an account at <https://hub.docker.com/> to host our Docker container
-   Note that you should run this function only when we are ready for our GitHub repository to be public. The free travis service we're using here requires your GitHub repository to be public. It will not work on private repositories. If you want to keep your GitHub repo private until after publication, you can use `rrtools::use_circleci()` for running free private continuous integration tests at <https://circleci.com>, instead of travis. With `rrtools::use_circleci(docker_hub = FALSE)` we can stop our Docker container from appearing on Docker Hub, so our Docker container stays completely private.

#### 8. `devtools::use_testthat()`

-   if you add functions in `R/`, include tests to ensure they function as intended
-   create tests.R in `tests/testhat/` and check <http://r-pkgs.had.co.nz/tests.html> for template

You should be able to follow these steps to get a new research compendium repository connected to travis and ready to write in just a few minutes.

References
----------

Kitzes, J., Turek, D., & Deniz, F. (Eds.). (2017). *The Practice of Reproducible Research: Case Studies and Lessons from the Data-Intensive Sciences*. Oakland, CA: University of California Press. <https://www.practicereproducibleresearch.org>

Marwick, B. (2017). Computational reproducibility in archaeological research: Basic principles and a case study of their implementation. *Journal of Archaeological Method and Theory*, 24(2), 424-450. <https:doi.org/10.1007/s10816-015-9272-9>

Piccolo, S. R. and M. B. Frampton (2016). "Tools and techniques for computational reproducibility." GigaScience 5(1): 30. <https://gigascience.biomedcentral.com/articles/10.1186/s13742-016-0135-4>

rOpenSci community (2017a). Reproducibility in Science A Guide to enhancing reproducibility in scientific results and writing. Online at <http://ropensci.github.io/reproducibility-guide/>

rOpenSci community (2017b). rrrpkg: Use of an R package to facilitate reproducible research. Online at <https://github.com/ropensci/rrrpkg>

Stodden, V. & Miguez, S., (2014). Best Practices for Computational Science: Software Infrastructure and Environments for Reproducible and Extensible Research. Journal of Open Research Software. 2(1), p.e21. DOI: <http://doi.org/10.5334/jors.ay>

Wickham, H. (2017) *Research compendia*. Note prepared for the 2017 rOpenSci Unconf. <https://docs.google.com/document/d/1LzZKS44y4OEJa4Azg5reGToNAZL0e0HSUwxamNY7E-Y/edit#>

Wilson G, Bryan J, Cranston K, Kitzes J, Nederbragt L, et al. (2017). Good enough practices in scientific computing. *PLOS Computational Biology* 13(6): e1005510. <https://doi.org/10.1371/journal.pcbi.1005510>

Contributing
------------

If you would like to contribute to this project, please start by reading our [Guide to Contributing](CONTRIBUTING.md). Please note that this project is released with a [Contributor Code of Conduct](CONDUCT.md). By participating in this project you agree to abide by its terms.

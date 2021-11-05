# Best practices for programming diagnostics

To facilitate code integration, code sharing between developers, reusability, easy
maintenance, and readability, it is recommended that these recommendations
are followed when developing new diagnostic scripts.

## Make your code open-source

It is recommended that you make your code
[open-source](https://the-turing-way.netlify.app/reproducible-research/open/open-source.html)
from the start, to facilitate reproducible science and encourage potential
[collaboration](https://the-turing-way.netlify.app/collaboration/collaboration.html).
If you have reservations about making your code open source before publication,
consider developing it in a private [git repository](#use-git-for-version-control) and making it
public once you have submitted your paper.

## Add a license

It is critical that you share your code with a
[license](https://the-turing-way.netlify.app/reproducible-research/licensing.html),
because others will not legally be allowed to use it if it does not have a license.
Make sure that any [dependencies](#use-libraries) you use or plan to use are
[compatible](https://the-turing-way.netlify.app/reproducible-research/licensing/licensing-compatibility.html)
with the license you choose.

## Use git for version control

It is recommended that you make use of a
[version control](https://the-turing-way.netlify.app/reproducible-research/vcs.html)
tool.
In particular, it is recommended that you use git, because of all available
version control systems it best facilitates (future) collaboration.

## Make your code citable

In order to get scientific credit for your work, you may wish to make it
[easy to cite your code](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-citation-files).
Many papers ask for a [DOI](https://www.doi.org/) for your code these days.
A convenient way to obtain that is by linking your
[GitHub](#host-your-code-on-a-website-with-an-issue-tracker) repository with
[Zenodo](https://zenodo.org/) as described in
[Making your code citable](https://guides.github.com/activities/citable-code/).

## Choose a suitable programming language

Most mature programming languages come with a set of standards, tools, and
services for developing and distributing software in that language.
Some programming languages come with an extensive ecosystem of libraries that
facilitate the analysis of climate data.
It is recommended that you choose a programming language for your diagnostic
script that has both of the above mentioned features.
At the moment, Python seems to be the most suitable language for analyzing
climate data, mostly because of the availability of suitable
[libraries](#use-libraries).

## Use libraries

Software that has been developed by a large and thriving community is usually
more reliable and faster than developing your own solution.
Therefore it is highly recommended that you make use of existing libraries
and contribute to existing software instead of developing your own software
from scratch.

Programming languages come with a large collection of built-in functions and
libraries, called a standard library.
It is recommended that you make use of functions from the standard library.
An example of such a library is the
[Python standard library](https://docs.python.org/3/library/).

Climate data is typically stored in NetCDF format using the CF-Conventions.
Usually climate data is larger than the amount of memory available in a typical
computer.
A programming language that is suitable for analyzing climate data has libraries
that support
- reading and writing NetCDF files or an equivalent storage format supporting the CF-Conventions
- understand the CF-Conventions
- computing statistics on arrays that are larger than memory
- creating figures

A good example of a programming language that meets all these requirements is Python,
where the [xarray](http://xarray.pydata.org) or
[iris](https://scitools-iris.readthedocs.io) libraries facilitate the processing
of large climate data, while [matplotlib](https://matplotlib.org/) or
[altair](https://altair-viz.github.io/) can be used for visualization.

## Follow code quality standards

It is recommended that you make use of the
[style guide](https://the-turing-way.netlify.app/reproducible-research/code-quality.html)
that is available for the programming language of your choice.
Examples of style guides:
- [PEP8](https://www.python.org/dev/peps/pep-0008/) and [PEP257](https://www.python.org/dev/peps/pep-0257/) for Python
- The [Tidyverse style guide ](https://style.tidyverse.org/)for R

If there is no style guide available for the programming language of your
choice, you may want to reconsider that choice because the language may not be
mature enough.
If there are compelling reasons to choose a language without a style guide,
it is recommended that you at least
- organize your code in multiple files of no more than a few hundred lines each
- organize your code in functions that fit your screen without scrolling (typically no more than 50 lines)
- use easy to understand variable names
- insert documentation or comments in your code that explain what is going on

This kind of organization increases readability of your code, thereby reducing
the risk of mistakes and making your code accessible to others.

Style guides typically come with automatic formatters, that can automatically
format your code according to the chosen style.
In addition to automatic formatters, tools for checking adherence to a style
guide as well as for common programming mistakes are usually also available.
These tools are called linters.
It is recommended that you make use of an automatic formatter and linter.
For example, a popular automatic formatter for Python is
[black](https://black.readthedocs.io), while a good linter for Python is
[pylint](https://pylint.org/).

## Keep your code reliable and maintainable
If you write large pieces of code, it is recommended that you implement some
form of
[automated testing](https://the-turing-way.netlify.app/reproducible-research/testing.html)
to ensure that you do not loose more and more time testing that your code still
produces the correct results as it grows.
This can range from
[unit tests](https://the-turing-way.netlify.app/reproducible-research/testing/testing-unittest.html),
that test each function in your code individually, to
[regression tests](https://the-turing-way.netlify.app/reproducible-research/testing/testing-acceptance-regression.html#regression-testing)
that run larger portions of code and just check that the changes you make to your
code do not change previously computed results that are not supposed to change.
For most programming languages, tools are available to run all tests with a
single command.
A good choice for running Python tests is [pytest](https://docs.pytest.org).

## Do code reviews
If at some point you find yourself working on code with multiple people, it is
highly recommended that you do
[code reviews](https://the-turing-way.netlify.app/reproducible-research/reviewing.html)
to improve the quality and reliability of your code.

## Use online services

A large number of services are available online to make developing and
distributing software easier.
There services are usually free for [open-source](#make-your-code-open-source)
projects.

### Host your code on a website with an issue tracker
It is recommended that you develop your diagnostic script
[in public](#open-source-with-a-license) on a modern code development platform
like [GitHub](https://github.com/) or [GitLab](https://about.gitlab.com).
This facilitates
[collaboration](https://the-turing-way.netlify.app/collaboration/collaboration.html)
and has the added benefit that you can make use
of services for automatically hosting
[documentation](#host-your-documentation-on-a-website),
[automatic quality control](#use-a-code-quality-checker),
[automatic testing](#automate-testing),
and [automatic publication](#use-a-package-registry).

Hold all conversations about developments of your code in the issue tracker,
to make your work accessible to users and potential collaborators.
If you have a conversation where decisions are made outside the issue tracker,
make sure to summarize it in an issue.

### Host your documentation on a website
The most convenient way to publish your documentation is a webpage.
It is recommended that you use the service for documentation that is the default
for the programming languages you are using.
For Python, [readthedocs](https://readthedocs.org/) is a common choice.

### Use a package registry

It is recommended that you only use libraries that are available in the default
package registry for the programming language of your choice, to ensure that
others are able to easily install the dependencies required to run your software.
Examples of package registries are the [Python Package Index](https://pypi.org/)
and the [Comprehensive R Archive Network](https://cran.r-project.org/) for
Python and R packages respectively.

Likewise, it is recommended that you distribute your diagnostic code as
(part of) a package in the default package registry.

### Use a code quality checker
Various services are available that can automatically find common mistakes
in your source code.
An example of such a service is [Codacy](https://www.codacy.com/).

### Automate testing
Many online services are available for running [tests](#keep-your-code-reliable-and-maintainable)
automatically whenever you make changes.
This is called [continuous integration](https://the-turing-way.netlify.app/reproducible-research/ci.html).
Examples of such services are
[GitHub Actions](https://github.com/features/actions),
[GitLab CI](https://docs.gitlab.com/ee/ci/introduction/index.html#continuous-integration),
or [CircleCI](https://circleci.com/).

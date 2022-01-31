# Motivation

## The current situation

The Earth Sciences community has a long record of openness and collaboration regarding libraries and supporting tools for climate science. This ranges from basic libraries like NetCDF or UDUNITS to complex frameworks like xarray, CDO, NCO and others.

The collaboration on tools is complemented with a huge effort on data sharing, with the CMIP project being its most important representative, and platforms like ESGF and the Climate Data Store providing access to huge amounts of observational and experimental data, along with the individuals efforts done by insititutions like NASA and ESA.

However, there is still a particular field in which collaboration can still improve: the actual pieces of code that analyze the data and produce the final results, hereafter called 'diagnostic scripts' or in short 'diagnostics' or 'scripts'. Those pieces of code are ones of the most expensive in term of climate scientists' development and tuning time, and the most sensitive with regard to scientific correctness, so collaboration will be a huge boon to the community.

That particular lack of collaboration is hindered by the sear amount of tools used by scientists that sometimes make difficult to use code developed in other institutions which use a very different software stack. This has usually meant that if a scientist wanted to apply the analysis from another colleague to a new case it had to reimplement at least part of it even if the original code was available.

## Basic assumptions about cross-plugging diagnostics among tools

From our experience, diagnostics can be organized as independent codes launched as independent processes. This is actually the case in tools like ESMValTool and CliMAF. It also assumes that such codes work from CF compliant NetCDF format data files and produce only the same kind of files alongside any final representations (images, documents, html pages...).

## Our objective

The objective of this standard is to provide a common interface to communicate with diagnostics
that can be used from nearly any language or tool that is available for climate analysis. The objective is that
each one of this diagnostics can be used as a standalone command line tool through this interface, allowing climate
scientists to plug them in the tool of their choice, or even to call them manually.

Forcing diagnostics to be independent also allows for a better management of conflicting software stacks,
as they can be run in isolated environments through any of the means currently available for that purpouse
(software modules, virtual environments, containers...). The standard, however, does not need to take any
of those solutions into account as they are easy enough to hide for the end user using command line scripts or aliases.

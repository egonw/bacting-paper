---
title: 'Bacting: a next generation, command line version of Bioclipse'
tags:
  - bioinformatics
  - cheminformatics
  - Bioclipse
authors:
  - name: Egon Willighagen
    orcid: 0000-0001-7542-0286
    affiliation: 1
affiliations:
 - name: Dept of Bioinformatics - BiGCaT, NUTRIM, Maastricht University
   index: 1
date: 19 July 2020
bibliography: paper.bib
---

# Summary

Bioclipse was orginally developed as an interactive user interface for research in the fields
of biology and chemistry based on Eclipse [@bioclipse1]. It was later extended with scripting
functionality and scripts could be written in JavaScript, Python, and Groovy [@bioclipse2].
An innovative aspect of the second Bioclipse version was that Bioclipse plugins could inject
domain specific functionality into the scripting language. This was done using OSGi and Spring
approaches, making so-called *managers* accessible in scripts.

# Statement of Need

However, the dependency of Bioclipse on the Eclipse UI requires the scripts to be run inside
a running Bioclipse application. This makes repeatedly running of a script needlessly hard
and use in continuous integration systems or on computing platforms impossible. A second
problem was that the build and release system of Bioclipse was complex, making it hard for
others to repeat creating new releases. For example, this complicates updating dependencies.

These two needs triggered a next generation design of Bioclipse: 1. the managers providing
the domain-specific functionality would need to be useable on the command line; 2. building
the Bioclipse managers should be possible on the command line, idealy with continuous build
systems; 3. Bacting should be easy to install and reuse.

# Implementation

To keep the option open to backport new functionality to Bioclipse, the API is copied as
precisely as possible. However, there are some differences. For example, there is only
a single manager class, and no longer interfaces for both the scripting language and for
the running Bioclipse user interface. This means that translation of *IFile* to *String*
translations in the API do not exist in Bioclipse. Furthermore, there are currently
no progress monitors. That said, the source code implementing the method is otherwise
identical and easily translated back to the original Bioclipse source code.

This is done by separating the Bioclipse code from the Bacting manager implementations.
The latter is mainly described in this paper, and the former is found as much more
stable code in the GitHub [https://github.com/egonw/bacting-bioclipse](https://github.com/egonw/bacting-bioclipse)
repository. The code is identical to the original Bioclipse code, but mavenized in
this repository, allowing deployment on Maven Central.

The Bacting manager are found in the [https://github.com/egonw/bacting](https://github.com/egonw/bacting)
repository and while the maangers in this repository share most of the code with the original
Bioclipse implementations, they are still considered new implementations and therefore
are tested using JUnit. A second important difference is that Bioclipse documentation was
found on the manager interfaces, but in Bacting the JavaDoc is found is found in the
implementations of the managers. A final difference is how the managers are used:
because they are not injected into the scripting language, each manager needs to be
created manually, which requires one extra line of code for each manager.

## Continuous integration and releases

Bacting is hosted on GitHub and takes advantage of the integrations with Zenodo for automatic
archiving of releases (see [https://github.com/egonw/bacting/releases](https://github.com/egonw/bacting/releases))
and with Travis-CI for continous integration (see [https://travis-ci.org/github/egonw/bacting](https://travis-ci.org/github/egonw/bacting)).
Maven is used as a build system and automatically downloads the dependencies when compiling the source code.
Travis-CI compiles the source code regularly with Java 8, 11, and 14. During the process the JUnit 5 unit
tests are run and the compilation aborted when there are testing failures.

Releases are made at irregular intervals, but coninciding with downstream uses. Releases are created
with the `mvn release:prepare` and `mvn release:performe` process that tags the commit, updates the
version numbers, and uploads the release to Maven Central. Second, a changelog is written for the
GitHub releases page, which triggers the archiving on Zenodo (see
[https://doi.org/10.5281/zenodo.2638709](https://doi.org/10.5281/zenodo.2638709)). Finally, at that
moment the JavaDoc is also generated and uploaded to another GitHub repository
(see [https://github.com/egonw/bacting-api](https://github.com/egonw/bacting-api))
making it online available with GitHub pages at [https://egonw.github.io/bacting-api/](https://egonw.github.io/bacting-api/).

## Updated dependencies of managers

The *cdk* manager wrapping Chemistry Development Kit functionality was updated to
version 2.3, released in 2017 [@Willighagen2017]. The *opsin* manager was
updated to use OPSIN version 2.4.0, released in 2018 [@Lowe2011]. The *bridgedb*
manager was updated the BridgeDb version 2.3.8, released in 2020 [@vanIersel2010].

## Availability 

The source code for ``Bacting`` has been archived to Zenodo with the linked DOI: [@zenodo].
Binary releases are also available from Maven Central at [https://search.maven.org/artifact/io.github.egonw.bacting/bacting](https://search.maven.org/artifact/io.github.egonw.bacting/bacting).

# Functionality

Bioclipse has a long list of managers and so far only a subset has been ported, focusing
in personal use cases, some of which are briefly described in this table:

| Bacting Manager      | Functionality                                                                        |
| -------------------- | ------------------------------------------------------------------------------------ |
| bioclipse            | Bioclipse manager with common functionality                                          |
| ui                   | Bioclipse manager with user interface functionality                                  |
| report               | Manager that provides an API to create HTML reports                                  |
| cdk                  | Chemistry Development Kit for cheminformatics functionality [@Willighagen2017]       |
| inchi                | Methods for generating and validating InChIs and InChIKeys [@Spjuth2013]             |
| pubchem              | Methods to interact with the PubChem databases                                       |
| chemspider           | Methods to interact with the Chemspider databases                                    |
| rdf                  | Resource Description Framework (RDF) functionality, using Apache Jena                |
| opsin                | Access to the OPSIN library for parsing IUPAC names [@Lowe2011]                      |
| bridgedb             | Access to the BridgeDb library for identifier mapping [@vanIersel2010]               |

The functionality of the Bioclipse managers is partly documented in the 
[A lot of Bioclipse Scripting Language examples](https://bioclipse.github.io/bioclipse.scripting/) booklet,
of which several scripts are available as Bacting examples. For example, the
[FullPathWikiPathways.groovy](https://bioclipse.github.io/bioclipse.scripting/code/FullPathWikiPathways.code.html)
page from this booklet shows both the Bioclipse version of the script as well as the Bacting version.

## Grabbing Bacting from Groovy

Use of Bacting in the Groovy language is taking advantage from the fact that it is available from Maven Central,
allowing `@Grab` to be use to dynamically download the code as in this example for the *cdk* manager:

```groovy
@Grab(group='io.github.egonw.bacting', module='managers-cdk', version='0.0.11')

def cdk = new net.bioclipse.managers.CDKManager(".");

println cdk.fromSMILES("COC")
```

# Use cases

The Bioclipse script have been in use in our group in various research lines to automate repetitive work.
Various scripts have now been ported to Bacting and several are now available as open notebook science
repositories at [https://github.com/egonw/ons-wikidata](https://github.com/egonw/ons-wikidata),
[https://github.com/egonw/ons-chebi](https://github.com/egonw/ons-chebi), and
[https://github.com/egonw/ons-wikipathways](https://github.com/egonw/ons-wikipathways). The scripts in these repositories are
used to populate Wikidata with chemical structures in the Scholia project [@Willighagen2018] and to support WikiPathways [@Slenter2018],
with OECD Testing Guidelines for the [NanoCommons](https://www.nanocommons.eu/) project, in to support the
creating of BridgeDb identifier mapping databases in an implementation study of the ELIXIR Metabolomics Community [@vanRijswijk2017].
Various of these use cases are ongoing and are not yet unpublished published, which is planned.

# Acknowledgements

We acknowledge the contributions of the Bioclipse developers which have been
ported here into the Bacting software.

# References

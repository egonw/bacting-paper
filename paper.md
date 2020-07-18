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
date: 18 July 2020
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
implementations of the managers.

## Updated dependencies of managers

The *cdk* manager wrapping Chemistry Development Kit functionality has been updated to
version 2.3, released in 2017 [@Willighagen2017]. The *opsin* manager has
been updated to use OPSIN version 2.4.0, released in 2018 [@Lowe2011].

# Functionality




The source code for ``Bacting`` has been archived to Zenodo with the linked DOI: [@zenodo].




# Acknowledgements

We acknowledge the contributions of the Bioclipse developers which have been
ported here into the Bacting software.

# References

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

# Functionality




The source code for ``Bacting`` has been archived to Zenodo with the linked DOI: [@zenodo].




# Acknowledgements

We acknowledge the contributions of the Bioclipse developers which have been
ported here into the Bacting software.

# References

.. contents::
   :depth: 3.0
..

Technical guide to the workshop
===============================

A lot of software and supporting libraries are necssary on a machine to
run the analysis used in the workshop. Many of these resources are
useable for general analysis of modern high-throughput sequencing data.

In the hopes it will be useful for workshop participants looking to
expand their knowledge and experience beyond what we could fit in today,
the following sections spell out the details of setting up such a system
on a Linux (Ubuntu Amazon Webs Services instance) and Mac. These were
the two systems on which this workshop was developed and therefore the
only ones for which I have first-hand knowledge to share. I apologize to
those on Windows systems, but you may find these guides useful as
general sign posts although you will have to look elsewhere for the
details of installing.

Software Utilized
-----------------

-  `Python <https://www.python.org/downloads/>`__
-  `BWA (Burrows-Wheeler Aligner) <http://bio-bwa.sourceforge.net/>`__
-  `SAMtools <http://www.htslib.org/>`__ OLD site:
   http://samtools.sourceforge.net/
-  `FASTX-Toolkit <http://hannonlab.cshl.edu/fastx_toolkit/download.html>`__
-  `libgtextutils <http://hannonlab.cshl.edu/fastx_toolkit/download.html>`__
-  `MACS2 <https://github.com/taoliu/MACS/>`__
-  `CEAS <http://liulab.dfci.harvard.edu/CEAS/download.html>`__
-  `BEDtools <http://bedtools.readthedocs.org/en/latest/content/installation.html>`__
-  `MEME webserver <http://meme-suite.org/tools/meme>`__

Suggested Other Software
------------------------

-  `FastQC <http://www.bioinformatics.babraham.ac.uk/projects/download.html#fastqc>`__
   <-- Not currently used here but good to have for initial assessment
   of reads. We will cover quality control throughout but not assess
   here ourselves.

.. contents::
   :depth: 3.0
..

Overview
========

General Overview of Workshop
----------------------------

::

    Get computer with software and data up and running
        |
        V
    Map the appropriate reads
        |
        V
    Call peaks with MACS
        |
        V
    Examine peaks relative to known genomic features using CEAS
        |
        V
    Motif discovery with MEME

More Detailed Overview of Workshop Pipeline
-------------------------------------------

::

    Get proper resources for working on genome of interest
        |
        V
    Identify proper data on NCBI Short Read Archive
        |
        V
    Obtain in a format that will work for subsequent steps (ideally fastq)
        |
        V
    Extract the Gal4 reads based on barcode
        |
        V
    Map the reads
        |
        V
    Improve the mapping of the reads
        |
        V
    Preliminary view of mappings
        |
        V
    Peak calling with MACS
        |
        V
    Peaks relative to known genomic features with CEAS
        |
        V
    Filter additional regions
        |
        V
    Compare to published data
        |
        V
    Motif discovery with MEME

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
   of reads. We will touch on quality control but not assess here
   ourselves.

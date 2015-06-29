.. contents::
   :depth: 3.0
..

STEP# 1: Get proper resources for working on genome of interest
===============================================================

It is typical that you get a basic set of genome data for any organism
with which you'll be working. By way of example, in preparation for
today we'll get a file that includes the sequence of the entire budding
yeast genome.

`Illumina's iGenomes
site <http://support.illumina.com/sequencing/sequencing_software/igenome.html>`__
is a collection of reference sequences and annotation files for commonly
analyzed organisms. Importantly, many of these are in multiple forms and
can save you conversions at times. You'll see today file conversions are
very common, and obviating the need for more can be a welcome
convenience.

We'll get the information that corresponds to the current version
available from the `UCSC Genome
Bioinformatics <http://genome.ucsc.edu/index.html>`__ for the budding
yeast *Saccharomyces cerevisiae*. Download
`sacCer3 <ftp://igenome:G3nom3s4u@ussd-ftp.illumina.com/Saccharomyces_cerevisiae/UCSC/sacCer3/Saccharomyces_cerevisiae_UCSC_sacCer3.tar.gz>`__
from the `iGenome
site <http://support.illumina.com/sequencing/sequencing_software/igenome.html>`__.

::

    wget ftp://igenome:G3nom3s4u@ussd-ftp.illumina.com/Saccharomyces_cerevisiae/UCSC/sacCer3/Saccharomyces_cerevisiae_UCSC_sacCer3.tar.gz

And unpack it.

::

    tar xzf Saccharomyces_cerevisiae_UCSC_sacCer3.tar.gz

(OPTIONAL) We'll clean up a bit by deleting the gzipped file we
downloaded. Plus the IGenomes README.txt also because soon we'll have
lots of files in our working directory.

::

    rm Saccharomyces_cerevisiae_UCSC_sacCer3.tar.gz
    rm README.txt

We'll need the sequence of the entire genome in fasta form and so we'll
pull that out of the large collection of files for yeast, copying it to
our current directory. The current directory is designated by the dot
Unix shortcut at the end of the command below.

::

    cp Saccharomyces_cerevisiae/UCSC/sacCer3/Sequence/WholeGenomeFasta/genome.fa .

We'll do likewise for another form of the genome information.

::

    cp Saccharomyces_cerevisiae/UCSC/sacCer3/Sequence/WholeGenomeFasta/genome.fa.fai .

(OPTIONAL) We have what we need but let's just put all the yeast data
somewhere out of the way so we have it if we need it but it isn't
cluttering our working directory.

::

    mv Saccharomyces_cerevisiae /usr

Verify when that is all set by checking directory.

::

    ls

Alternatively, you can use
`rsync <http://genome.ucsc.edu/goldenpath/help/ftp.html>`__ to go obtain
these files directly from `UCSC Genome
Bioinformatics <http://genome.ucsc.edu/index.html>`__.

In a browser you can download
`here <http://hgdownload.cse.ucsc.edu/downloads.html>`__ or do `via FTP
here <ftp://hgdownload.cse.ucsc.edu/goldenPath/>`__. The fasta file from
there seems to require additional conversion, see the about
``twoBitToFa``
`here <http://hgdownload.cse.ucsc.edu/goldenPath/sacCer3/bigZips/>`__

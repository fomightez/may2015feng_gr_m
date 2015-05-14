.. contents::
   :depth: 3.0
..

Making Wiggle file Covering Genome
==================================

One of the required files for the ``build_genomeBG`` utility included
with the CEAS package is a wiggle file that describes the genomic
locations that are valid for data selection. For example, a tiling array
may only cover some parts of a genome. In our case, we theoretically
should have covered all the *Saccharomyces cerevisiae* genome and so we
simply generate a wiggle file that covers the full length of each and
every chromosome.

For example a tiling array may only cover some parts of the genome. In
our case we simply generate a `wiggle
file <http://genome.ucsc.edu/goldenpath/help/wiggle.html>`__ that covers
the full chromosomes. Start will be one for each and every chromosome
and the ``step`` will simply be the length of the chromosome. A
convenient place to mine that information is to look at the header for
the SAM file we generated earliey in our pipeline. The lines begining
``@SQ`` have all this information. So you'd build of the lines below in
your text editor using the length of the chrosomomes to make your own
file. Looking at the file you are making, the first few lines will be
like so and continue on to cover all the chromosomes, included the
mitochondria.

::

    track type=wiggle_0
    fixedStep chrom=chrI start=1 step=230218
    1
    fixedStep chrom=chrII start=1 step=813184
    1
    fixedStep chrom=chrIII start=1 step=316620
    1
    fixedStep chrom=chrIV start=1 step=1531933
    1

    ... etc ...

This can actually be done right in the command-line using utilities like
``nano`` and ``vim``.

For the sake of time today, the result is made available for you already
`here <https://raw.githubusercontent.com/fomightez/may2015feng_gr_m/master/sacCer3.wig>`__.

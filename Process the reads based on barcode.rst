.. contents::
   :depth: 3.0
..

Extract the Gal4 reads based on barcode
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Note: Generally, most times the sort of processing (demultiplexing based
on barcodes or indexing) described in this section will already have
been done in modern datasets you either receive from a facility or find
publically.

According to metadata at `http://www.ncbi.nlm.nih.gov/sra/SRX098212[accn]
<http://www.ncbi.nlm.nih.gov/sra/SRX098212[accn]>`__

    Experiment2 (SSFs 1). Barcoded paired-end run was performed in
    SRX098212 by AB SOLiD System 3.0. 35 bp F3 (mate 1) run contains
    genomic sequences. 10 bp R2 (mate 2) run contains 6 bp barcoded
    sequences (first 6 bp are barcode in 10 bp R3 paired-end sequences).
    SRX098212 contains the following 2 runs with indicated barcode
    sequences: Run1. SRR346368 (1.8 Gb) contains two samples: ChIP-exo
    Reb1 (biological replicate 1, GGGCTT) and ChIP-exo Gal4 (biological
    replicate 1, TAGCGT).

We only want to map the Gal4 reads in our subsequent analysis. Hence, we
want to sort out and make a new file of the genomic sequences
``F3 (mate 1)`` that match the Gal4 barcode ``TAGCGT``. Thus removing
those that correspond to the Reb1 data.

The journal article was unclear how they did this step so I made a
custom Python script to implement this approach. One thing to keep in
mind about this program is that it is very stringent in that the barcode
has to be an exact match at the start of ``mate 2``.

We'll get the script from among a batch of sequence analysis scripts I
have at Github.

::

    git clone https://github.com/fomightez/sequencework.git

Then to make it easy we'll copy the particular script into our current
working directory, which we'll specify via ``.`` in the line below.

::

    cp sequencework/SOLiD/split_SOLiD_reads.py .

With the data and the script in the same directory, enter on the command
line

::

    python split_SOLiD_reads.py SRR346368.fastq TAGCGT 10

It will take about 15 to twenty minutes to run.

For the sake of time, we are not going to concern ourselves much with
the details of the script today. If you want to continue your
exploration of coding with Python that we started in the first session,
I'd suggest you look over the implementation of the sorting described
above
`here <https://github.com/fomightez/sequencework/blob/master/SOLiD/split_SOLiD_reads_basic.py>`__.
The main code is way at the bottom, and keep in mind this is a fairly
advanced script only because it includes several bells and whistles,
such as a built-in debugging option, a usage guide, and reading
arguments from the command line. The link is actually for the code for a
simpler version of the script we used today. The one we used has an
extra check built in to make sure the sets of reads always matched up
based on the annotation for each read. The documentation for both
scripts can be found
`here <https://github.com/fomightez/sequencework/tree/master/SOLiD>`__.

Upon completion of the script running and processing the data, you
should see something very similar to below. Make sure your collected
reads count is similar (hopefully, even exactly the same).

::

    root@ip-10-169-123-185:/usr/workshop# python split_SOLiD_reads.py SRR346368.fastq TAGCGT 10
    Reading in your FASTQ file...
    Concluded.
    46798614.0 read pairs analyzed. A grand total of 93597228 reads.
    180054 reads collected based on barcode.
    The file SRR346368_TAGCGT.fastq has been created in same directory as the input file.

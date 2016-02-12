.. contents::
   :depth: 3.0
..

STEP #2: Acquire experimental data
==================================

Identify proper experimental data
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We are going to use data from this publication today.

::

    Rhee HS and Pugh BF. 2011. Comprehensive genome-wide protein-DNA
    interactions detected at single-nucleotide resolution.
    Cell. 2011 Dec 9;147(6):1408-19. doi: 10.1016/j.cell.2011.11.013.
    PMID: 22153082 http://www.ncbi.nlm.nih.gov/pubmed/22153082.

-  ``Footnotes`` section of
   `paper <http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3243364/#SD1>`__
   contains an ``ACCESSION NUMBERS`` section that provides us with the
   accession ``SRA044886``.

   That leads to `NCBI Sequence Read
   Archive(SRA) <http://www.ncbi.nlm.nih.gov/sra/>`__ where similar to
   the process of looking up Genbank sequence entry with an accession.
   So at the SRA you'd search the accession ``SRA044886`` similarly.

   Enter on the search box ``SRA044886``.

   Leads us `here <http://www.ncbi.nlm.nih.gov/sra?term=SRA044886>`__

   Leads to 5 experiments with the last on the list, `SSFs
   1 <http://www.ncbi.nlm.nih.gov/sra/SRX098212[accn]>`__ being, the one
   with Gal4 reads in which we are interested today.

   Finally, you'll see two runs are listed on the bottom of that page,
   and if you were to parse the cryptic metadata at the top of the page,
   you'd see the specific run in which we are interested is
   ``SRR346368``.

-  Alternatively, the ``READ ME`` at
   http://downloads.yeastgenome.org/published\_datasets/Rhee\_2011\_PMID\_22153082/
   leads to ``SRA044886`` at SRA as well.

Obtain file of the experimental data in a workable format
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When I first downloaded naively trying to click things in the browser,
it downloaded as an SRA file.

NCBI regards the SRA file format as a basic format that saves space and
allows conversion to several popular formats.

Going by my recent experience developing the pipeline for this workshop,
I strongly suggest you always use the ``SRA-Toolkit``, see `Converting
SRA format data into
FASTQ <http://www.ncbi.nlm.nih.gov/books/NBK47540/#SRA_Download_Guid_B.Downloading_SRA_Data>`__
to acquire and convert your data. The SRA handbook is
`here <http://www.ncbi.nlm.nih.gov/books/NBK242621/>`__. Presently, you
can find `the SRA Toolkit Installation and Configuration Guide
here <http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=std>`__.
I strongly advise you to follow `those
instructions <http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=std>`__
in step-by-step conjuction with the technical details of installation
for installing on a Mac and EC2 instance the instructions exactly,
trying the ``./fastq-dump -X 5 -Z SRR390728`` test before doing any
troubleshooting or configuring. The `NCBI SRA toolkit
instructions <http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=std>`__
as they are written in April 2015 seem to skip a step and assume you
really understand how to specify absolute paths on your system. Pointing
the program file with the abolsute path is critical to getting it
working on your system. And the step they skip is placing the contents
of the ``bin`` directory in a directory you set up; the instructions in
the technical details of setting up the SRA toolkit on a Mac and EC2
instance include this VERY IMPORTANT STEP.

You will see other, older posts on dealing with getting SRA files and
converting to fastq or going directly to fastq, such as
[here](https://www.biostars.org/p/128831/. Bear in mind he NCBI has been
actively developing the SRA toolkit a while now and things have
progressed making a lot of the information out there outdated. What
worked in the past may not work now. The SRA toolkit is what NCBI has
been emphasizing as the singular approach for obtaining data files. For
example, if you can go through steps in `advice under post#7
here <http://seqanswers.com/forums/showthread.php?t=8220>`__ to try and
download select the fastq format using check boxes on a web form, you'll
see you be presented with directions about using the SRA toolkit now.
Similarly, despite old posts advising it, if you try to use curl or wget
to download a link to reads on the command line, all you'll receive is a
short note to use the SRA toolkit.

From experience I can tell you this is a very real issue and you might
not know it when you are faced with massive files and determining
absolute numbers and things isn't easy and troubleshoot when you don't
necessarily know what you should have at the end when the process works
or what form is best for going forward. This will become evident when we
deal with the options needed to get today's data.

Obtaining a fastq file using the SRA toolkit
''''''''''''''''''''''''''''''''''''''''''''

Go to the working directory; this will be the User system resources
directory of our Amazon instances for no other reason except it will
allow me to set the instances in a so-called ``stop`` mode that is
cheaper to maintain. It is more like a ``pause`` mode because you can
``restart`` the instance. This way if you have questions a the end of
the workshop, for the next few days we'll have the option to keep your
instance exactly as you left it. (If we did it in the scratch disk area
of the drive ``/mnt``, which is otherwise a perfectly suitable place to
work, all the contents would disappear if I ``stop`` the workshop
instances. Then we'd be back at square one.) I don't plan to terminate
them immediately. (Termination would destroy everything.)

We are going to make a subdirectory there to keep better organized.
We'll call the subdirectory ``workshop``.

::

    cd /usr
    mkdir /workshop

Enter

::

    ~/sratoolkit/bin/fastq-dump -B -I --split-spot --minReadLen 0 SRR346368

The download and conversion to fastq format trigger by the command
should take a few minutes to run. Let it go.

Exploring the command
                     

The path in front of ``fastq-dump`` is to point at a special location
when I put this in program for convenience so I could stop and restart
EC2 instance without needing to redownload the software. You generally
could put this somewhere more useful and easily accessible.

What do the options mean? (You can type
``./sratoolkit/bin/fastq-dump --help`` to see the full list of other
options.)

-  The ``-B`` option is `covered
   here <http://www.ncbi.nlm.nih.gov/books/NBK56563/#SRA_download.fastqdump_outputs_color_spa>`__.
   It will yield you the fastq data as bases and not other
   representative symbols. Generally in newer data you do not need this
   option, but our data here is not from the lastest Illumina pipelines
   and in fact is old and from a different (yet `still
   used <http://journals.plos.org/plosone/article?id=10.1371/journal.pone.0126289>`__,
   `surprisingly to
   some <https://twitter.com/pathogenomenick/status/593429011327496192>`__)
   technology, the Applied Biosystem, SOLiD.

    Fastq files that have numbers instead of letters are in color space
    format. This format is a feature unique to the methodology used by
    the ABI SOLiD sequencer. ... In order to dump a “normal” base space
    fastq file, please run the fastq-dump command with the “-B” option.

-  The -I option adds read id after spot id as ``accession.spot.readid``
   on defline of the read information. Just accept at this point it will
   make things easier to keep track of the mates in the read pairs. You
   may find this option convenient for other public data you access.

-  Similar to the prior two options, ``--split-spot`` option generally
   will not be necessary for most modern datasets, but it is critical
   for the way this older form this older SOLiD data form is stored (as
   a single spot with a read for the genomic data and a read for the
   barcode) and for the next steps we are going to take with this data
   to filter it to only the reads for which we are concerned today. For
   modern data, the somewhat related ``--split-files`` option is
   commonly used when dealing with paired-end data and results in each
   pair of ends in a separate file, usually resulting in two files names
   similar to ``SRRXXX_1.fastq`` and ``SRRXXX_2.fastq``. For data you
   are receiving directly from a facility it will probably already have
   been handled this way and hopefully they will disclose this
   information to you or have an FAQ describing how the indexed (or
   barcoded) and paired-end reads have been handled.

-  ``--minReadLen`` of zero is designated so all reads will be kept.
   This is also to facilitate the next step dealing barcode by helping
   insure same number of lines for each and every set of reads. Again,
   this is not an option you'd typically use.

(As others have pointed out (`for
example <http://blastedbio.blogspot.co.uk/2011/10/fastq-must-die-long-live-sambam.html>`__,
splitting the paired-end reads into two files actually goes against the
``tidy data`` idiom of keeping associated data in a single file, but it
is commonly done for convenience. A number of `processing and analysis
programs expect the data in the separated
form <http://seqanswers.com/forums/showpost.php?p=129878&postcount=7>`__.
The combined form is sometimes referred to ``megred`` or ``interleaved``
or ``interwoven``. There are alternate ways to go back and forth between
the two forms, see `split-pe.py
here <http://ged.msu.edu/angus/2013-04-assembly-workshop/getting-data.html>`__
and
`interleave.py <http://ged.msu.edu/angus/2013-04-assembly-workshop/trimming-and-quality-evaluation.html>`__
or `here <https://www.biostars.org/p/19446/>`__ or
`SeqPrep <https://github.com/jstjohn/SeqPrep>`__ for examples, but as
you can imagine,\ `best to handle this delicately as you have to hope
there are no hiccups. Sorting out if there were or not can be a
challenge <https://www.biostars.org/p/59707/>`__ with huge files with
millions of reads involved. Plus as you'll soon experience `files
multiply <http://omicsomics.blogspot.com/2012/12/the-trouble-with-fastq.html>`__.)

Ideally we should have just been able to type
``./sratoolkit/bin/fastq-dump SRRXXXXXXX`` or maybe
``./sratoolkit/bin/fastq-dump --split-files SRRXXXXXXX``. With data you
find publically it may take a bit of sleuthing to determine how it is
stored and then decide how to best fit it into your analysis pipeline.
The options used here today for the ``fastq-dump`` are a result of
considering that.

Results of the command
                      

The download and conversion should a good maybe 40 minutes or upwards to
run. (This is why I set up the systems some in advance on the day of the
actual workshop.)

When done, you should see

::

    Read 46798614 spots for SRR346368
    Written 46798614 spots for SRR346368

And you'll have a file ``SRR346368.fastq`` in your directory.

If you type

::

    ls -lah

you'll see the file is around 18 Gb.

Additional help for getting your data from the Short Read Archive
                                                                 

::

    * [Several solutions from early 2015](http://genomespot.blogspot.com/2015/01/sra-toolkit-tips-and-workarounds.html)

    * Use ENA as described in a 2013 post [here](http://seqanswers.com/forums/archive/index.php/t-19848.html)

        You go to the [European Nucleotide Archive](http://www.ebi.ac.uk/ena) and search for your data. For example, `SRR346368` in `text` search.
        Then drill down by clicking on 'Run 1' and then [you'll see a table](http://www.ebi.ac.uk/ena/data/view/SRR346368) with 'Fastq files (ftp)'
        The odd thing is that for this example the ENA approach didn't seem helpful. First, there are three files when by contrast there is only one for this run at NCBI SRA. Second, turns out these are colorspace format?!?! Puzzling is that in the middle of the three files there is 35 length sequences with many 10s in the middle and then more 35 bp length ones at end. Plus the reads seem out of order if you simply look at the first 10 lines of the first file. So just be aware you may need to look around at the possible resources to see what provides the most useful form of the dataset.

Two take away lessons: (1) when using public data, you may need to try
several forms to get what is easiest to work with for you. (2) When
working with millions of reads, it can be easy to miss small details
that could have huge effects downstream. For example, depending on how
subsequent steps are performed, such as demultiplexing by barcode,
tossing out of the zero length reads could bring things out of sync and
result in reads getting sorted completely wrong.

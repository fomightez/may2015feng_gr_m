.. contents::
   :depth: 3.0
..

Mapping reads
-------------

There are several steps involved in mapping our sequence reads and
getting the output into a usable form. First we need to tell bwa to make
an index of the reference genome. This is a way to set up the genome
data in a way that will be computationally efficient for accessing.
You'll see this is a common theme in bioinformatics pipelines.

::

    bwa index genome.fa

Now we are ready to do the mapping. It takes two commands. The
``bwa aln`` command aligns the reads and produces a file of all the
possible candidates. ``bwa samse`` takes that file and the knowledge it
is single-end sequencing to process the alignment data to produce the
resulting SAM file where repetitive hits will be randomly chosen.

::

    bwa aln genome.fa SRR346368_TAGCGT.fastq > SRR346368_TAGCGT_Ys_genome.sai

    bwa samse genome.fa SRR346368_TAGCGT_Ys_genome.sai SRR346368_TAGCGT.fastq > SRR346368_TAGCGTvsYsGenome_bwa.sam

See `manual <http://bio-bwa.sourceforge.net/bwa.shtml>`__ and `Biostars
question on bwa alig and bwa
samse <https://www.biostars.org/p/59572/>`__ or `Biostars question on
what is a .sai file <https://www.biostars.org/p/17311/>`__ for a full
and two quick guides, respectively, to what is going on with running the
two commands.

Note if you are working with paired-ends you'd use ``bwa sampe`` to
generate your SAM file, providing the information for both reads in your
command. This would use the file created by ``bwa aln`` and the
knowledge that it is paired-end sequencing to select the best hits
according the orientations, distance between the reads, etc.. An
unrelated example command for paired-ends is below as an EXAMPLE. NOT
FOR RUNNING TODAY.

::

    bwa sampe dmel-all-chromosome-r5.37.fasta RAL357_1.sai RAL357_2.sai RAL357_1.fastq RAL357_2.fastq > RAL357_bwa.sam

BWA is able to take advantage of multi-threading and so you'll get more
speed if you have multiple cores on your computer which many have. For
example, my iMac at home has two cores while my iMac at work has four
and several of Amazon's Elastic Cloud computing instances have 4 or more
cores as summarized `here <http://www.ec2instances.info/>`__. The
example we chose for today has few reads to map and so it is fast
without running on multiple threads. However, if you had a lot of reads
it would be preferable to run the alignment on multiple threads at the
same time using your machines multi-core processor. To do so you use the
-t option to specify the number of threads. For example, to run in four
simultaneous threads:

::

    bwa aln -t 4 genome.fa SRR346368_TAGCGT.fastq > SRR346368_TAGCGT_Ys_genome.sai

(You can use a UNIX trick to let the computer determine how many cores
it has and thus how many threads it can run. On Linux, replace ``4`` on
the above line with ``$(nproc --all)`` as illustrated
`here <https://jbadomics.github.io/tnseq/#tn-seq-data-analysis-workflow>`__.
On a Mac, you'd replace ``4`` with ``sysctl -n hw.ncpu``.)

Now look at the SAM output file

::

    head -100 SRR346368_TAGCGTvsYsGenome_bwa.sam

As summarized
`here <http://fg.cns.utexas.edu/fg/course_notebook_chapter_seventeen.html>`__

    The first four columns, in order, are: identifier, flag, chromosome,
    position, map-quality.

One of the key pieces of information is found in the second column. See
`here <http://seqanswers.com/forums/showthread.php?t=17314>`__ for a
nice summary of how to interpret it for unpaired reads. For paired reads
interpreting is more complex as described there as well. For full
details of the SAM format specification see
`here <https://samtools.github.io/hts-specs/SAMv1.pdf>`__. A nice primer
can be found `here <http://biobits.org/samtools_primer.html>`__.

We see a lot of '4's indicating only a few mapped. Can we get some
feedback easily? We can if we use the SAMtools software to look at the
SAM file. Type:

::

    samtools

You'll see a listing that includes:

::

    flagstat    simple stats

That looks promising. As described `in post #5
here <http://seqanswers.com/forums/showthread.php?t=16500>`__ it isn't
documented in `the
manual <http://www.htslib.org/doc/samtools-1.2.html>`__. Let's try
``flagstat`` anyways.

::

    samtools flagstat SRR346368_TAGCGTvsYsGenome_bwa.sam

We get:

::

    EOF marker is absent. The input is probably truncated.
    [bam_header_read] invalid BAM binary header (this is not a BAM file).

Plus a bunch of ``zero`` results.

Despite the name, it seems it needs a BAM file to do this. Well, a BAM
file is just a compressed binary version of a SAM file according to `the
list of Data File Formats at UCSC Genome
Bioinformatics <https://genome.ucsc.edu/FAQ/FAQformat.html#format5.1>`__.

The `first example under Examples on SAMtools documentation
page <http://www.htslib.org/doc/samtools-1.2.html>`__ (WAY AT THE
BOTTOM) illustrates what we'd like to do.

::

    samtools view -bS SRR346368_TAGCGTvsYsGenome_bwa.sam > SRR346368_TAGCGTvsYsGenome_bwa.bam

(According to `here <http://linux.die.net/man/1/samtools>`__ the ``-S``
tag for ``samtools view`` had in the past been used to be to specify
that the input is in the SAM format. That flag looks to be unnecessary
now `according to the doccumentation for the updated
version <http://www.htslib.org/doc/samtools-1.2.html>`__. The way shown
above should work no matter what version of SAMtools is on your system.)

Now since it is a binary and compressed version of a SAM file, unlike
the SAM file we just examined, a BAM file is not human-readable in any
way. If you need convincing, just type:

::

    head SRR346368_TAGCGTvsYsGenome_bwa.bam

However, ``samtools flagstat`` hopefully will work with this now. Try:

::

    samtools flagstat SRR346368_TAGCGTvsYsGenome_bwa.bam

Only 4.44% mapped?!??!

Improving the mapping of the reads
----------------------------------

Let's review the methods and see how it compares to what we did.
(Fortunately, in this case they detailed parts of the computational
analyses right in the supplemental information of the paper. You won't
always be this lucky.)

From the supplemental methods information in paper:

    Alignment to genome, peak calling, and data sharing. The
    Saccharomyces reference genome was obtained from www.yeastgenome.org
    (build: 19-Jan-2007). The entire length of the sequenced tags were
    aligned to the reference genome using Corona Lite software provided
    by the SOLiD system, allowing up to 3 mismatches. This process was
    repeated for the remaining tags, after removal of the 3’ most 6 bp,
    which tend to have higher error rates.

As noted in the supplemental methods information they allowed mismatch
to be 3, the `default for BWA listed as the first
option <http://bio-bwa.sourceforge.net/bwa.shtml>`__ is 4% of read
length which is much smaller than 3. So let's try what they did by
setting that ``-n`` option we just examined.

::

    bwa aln -n 3 genome.fa SRR346368_TAGCGT.fastq > SRR346368_TAGCGT_Ys_genomeN3.sai

    bwa samse genome.fa SRR346368_TAGCGT_Ys_genomeN3.sai SRR346368_TAGCGT.fastq > SRR346368_TAGCGTvsYsGenome_bwaN3.sam

Let's convert that to BAM and look at the stats.

::

    samtools view -bS SRR346368_TAGCGTvsYsGenome_bwaN3.sam > SRR346368_TAGCGTvsYsGenome_bwaN3.bam

    samtools flagstat SRR346368_TAGCGTvsYsGenome_bwaN3.bam

Indeed, improvement. Nearly 6% now.

The supplemental methods for the paper go on to describe that they took
these unmapped reads and cut off the last six and then ran the mapping
with those again. Obviously they are sharing some of the results if
their quality assessment here and this is in part while we didn't
address earlier. We skipped running FastQC as the very first step in the
interest of time and because they reported related information.) How
would we do such cutting off of bases?

As you probably guessed, with **more software**.

First we need a way to get the unmapped reads. To get them, we'll again
rely on SAMtools.

::

    samtools view -f 4 SRR346368_TAGCGTvsYsGenome_bwaN3.bam > SRR346368_TAGCGTvsYsGenome_bwaN3.unmapped.sam

That `filters the reads that have ``4`` for the flag
value <http://seqanswers.com/forums/archive/index.php/t-16743.html>`__.
(We won't need the @SQ header for this file so we left off the ``-h``
option.) You can look at the output using the ``head`` command.

The filtering created a SAM file with the unmapped, but we need to
supply bwa with a fastq file for the mapping step. As described
`here <https://www.biostars.org/p/770/>`__, you have options for doing
this. We are going to take advantage of a `unix utility called
Awk <https://davidlyness.com/post/the-functional-and-performance-differences-of-sed-awk-and-other-unix-parsing-utilities>`__
to do this, adapting the solution `described
here <http://www.cureffi.org/2013/07/04/how-to-convert-sam-to-fastq-with-unix-command-line-tools/>`__.

::

    grep -v ^@ SRR346368_TAGCGTvsYsGenome_bwaN3.unmapped.sam | awk '{print "@"$1"\n"$10"\n+\n"$11}' > SRR346368_TAGCGTvsYsGenome_bwaN3.unmapped.fastq

The description of the approach nicely breaks down the relevant steps.
One thing it assumes is you know ``|`` stands for using pipes to pass
the output from one command to another's input. The `Practical Computing
for Biologists book <http://practicalcomputing.org/>`__ I recommended
covers this and redirects if you need guidance about pipes with a
biological bent beyond what the internet provides.

You can look at the output using the ``head`` command and see we have
our fastq file of unmapped reads.

As for what software for trimming the reads... this is one of those
processes where a lot of solutions exist. You could even build a
customized one. For example, the Python program we used earlier to sort
based on barcode could easily be copied and adapted to make a new
program that takes every read and collects all except the last 6 bases
and outputs it to a new file. One of the most popular set of tools for
post-processing raw short reads for downstream approaches is the
`FASTX-Toolkit from the Hannon
lab <http://hannonlab.cshl.edu/fastx_toolkit/>`__. There are web-based
versions within Galaxy and command line versions. It is a handy set of
programs to be able to run and so we'll use a program from the
collection today for trimming,
`fastx\_trimmer <http://hannonlab.cshl.edu/fastx_toolkit/commandline.html#fastx_trimmer_usage>`__,
as having the tools installed will present additional post-processing
abilities to you, including `a more full-featured approach to
demultiplexing based on
barcodes <http://hannonlab.cshl.edu/fastx_toolkit/commandline.html#fastx_barcode_splitter_usage>`__.

The Fastx toolkit requires you install libgtextutils first. Once you
have those installed you are ready to trim. (I have installed them for
you today.)

::

    fastx_trimmer -l 29 -i SRR346368_TAGCGTvsYsGenome_bwaN3.unmapped.fastq -o unmapped.trimmed.fastq

Now we have the unmapped reads in a trimmed form. To make the steps
after easier, let's take the untrimmed reads that mapped previously and
append them to the unmapped reads in a trimmed form.

Since we saw before that filtering on the 4 flag worked, `the solution
to getitng the mapped reads <https://www.biostars.org/p/56246/>`__ is to
instead keep all WITH THE EXCEPTION OF the unmapped.

::

    samtools view -F 4 SRR346368_TAGCGTvsYsGenome_bwaN3.bam > bwaN3.mapped.sam

(Note that one solution you may have thought of is that you could
separately collect those that mapped to the forward and the reverse
using the ``-f`` option to filter on ``0`` and ``16``, respectively.
DON'T. While it will work for filter on ``16`` to get those mapped to
the reverse, it fails to filter out anything if you try to filter on
``0``. Perhaps counterintuitively, to get those
``forward mapping reads`` you can see in the SAM file as having a ``0``
value for the flag, `you need to actually use ``-F 20`` as the option
setting <https://www.biostars.org/p/14378/>`__. The reason the filter on
``0`` approach fails and the ``F -20`` option works is because the `flag
is bitwise <https://www.biostars.org/p/7374/>`__ so that the values can
be combined to express complex sets in a compact,'computer
science-wisey' way. `This tool
here <https://broadinstitute.github.io/picard/explain-flags.html>`__ is
particularly helpful for novices deciphering this. (There is now a tool
that claims to improve upon the Broad's utility
`here <http://djf604.github.io/SAM-flags-explained-improved/>`__) In
this tool's ``flag`` box, enter the flag value of ``20`` and hit
``explain`` you see that is a way of expressing those that are unmapped
or map to the reverse strand. With ``-F`` option we exclude those. You
may wish to seek additional help on this concept by looking at `slides
28-31 of this
presenation <http://www.slideshare.net/lindenb/ngsformats?ref=http://plindenbaum.blogspot.com/2013/09/presentation-file-formats-for-next.html?m=1>`__
and information and links `here <https://www.biostars.org/p/52657/>`__
or `here <https://www.biostars.org/p/107256/>`__ or `here as mentioned
earlier <http://seqanswers.com/forums/showthread.php?t=17314>`__ or
`swbarnes' answer
here <http://seqanswers.com/forums/archive/index.php/t-21843.html>`__.)

Again, we need to convert these to fastq as we did before.

::

    grep -v ^@ bwaN3.mapped.sam | awk '{print "@"$1"\n"$10"\n+\n"$11}' > bwaN3.mapped.fastq

Now that we have a fastq file of just the mapped reads we are going to
start a ``combined`` file and append those to the unmapped. We'll use
the redirection symbol ``>>`` to designate appending.

::

    cat unmapped.trimmed.fastq > combined.fastq

    cat bwaN3.mapped.fastq >> combined.fastq

Finally, we should be able to emulate the mapping approach used in the
paper.

::

    bwa aln -n 3 genome.fa combined.fastq > combined_Ys_genomeN3.sai

    bwa samse genome.fa combined_Ys_genomeN3.sai combined.fastq > combined_vs_YsGenome_bwa.sam

The first line of that should take about three minutes.

And convert to a BAM file so we can use flagstat.

::

    samtools view -bS combined_vs_YsGenome_bwa.sam > combined_vs_YsGenome_bwa.bam

So how did we do?

::

    samtools flagstat combined_vs_YsGenome_bwa.bam

Alright, 13% mapped is a big improvement over the 6% we had. But
normally you might expect better. I have communicated with Ho Sung Rhee
and he says this data was from the "early days" and was concerned about
a particularly high error rate. High throughput requires a different
mindset than a lot of the traditional molecular biological data we
collect. A lot of times you are fighting tooth and nail to get a signal,
and are concerned what you want to see was lost along the way. We still
have A LOT of data here even though we only around 10% of what we
corralled up to this point is mapping. A lot of what Titus Brown and
colleagues do does in processing of metagenome assembly data is data
reduction approaches. Maybe the ChIP-exo processing steps combined with
our strigent requirements of the barcode resulted in a reduction of data
that benefits us ultimately? Subsequent analysis will let us evaluate
the data in this case.

Preliminary view of mappings
----------------------------

So far we have just looked at the mappings in a list. SAMtools has a
program called ``tview`` that can help us get a sense visually of what
we have done so far.

What does ``tview`` need? Let's check `the
manual <http://www.htslib.org/doc/samtools-1.2.html>`__. I'd suggest
searching ``tview`` on the page because oddly the documentation is not
internally linked and indexed.

We see need to do yet some further file formatting to get the data in a
usable form for ``tview``.

In order to be able to use our most recent mapping results with
``flagstat`` we already converted it into a BAM file. Now we need a
**sorted** BAM file.

::

    samtools sort combined_vs_YsGenome_bwa.bam combined_vs_YsGenome_bwa.sorted

Okay, check your file listings.

::

    ls

Now that we have that sorted BAM file, let's TRY to use that to run
``tview``. Recall from the
`documentation <http://www.htslib.org/doc/samtools-1.2.html>`__ we need
a fasta formatted reference sequence too.

::

    samtools tview combined_vs_YsGenome_bwa.sorted.bam genome.fa

Okay, so this is going to cause SAMtools to give you feedback.

::

    [bam_index_load] fail to load BAM index.
    Cannot read index for 'combined_vs_YsGenome_bwa.sorted.bam'.

This is telling us we need to format yet one more type of file
concerning the data. We need to index the data. If you search on
``index`` in the `SAMtools
documentation <http://www.htslib.org/doc/samtools-1.2.html>`__ you'll
see there is a command to do this and that it is needed for commands
``when region arguments are used``. Going back to the documentation for
``tview`` you'll see ``tview`` can use these. *Lesson: the documentation
may not always explicitly say what you need where you might think it
would be best placed and error/feedback messages will often guide you.*

Let's index our sorted bam file.

::

    samtools index combined_vs_YsGenome_bwa.sorted.bam

You'll see this creates a file ending
``combined_vs_YsGenome_bwa.sorted.bam.bai`` with ``.*ai`` at the end
indicating SAM/BAM index file and if you look at the ``samtools index``
documentation you'll the purpose is to enable fast access by ``tview``
to the coordinate-sorted data. Creation if an index file (or lookup
table, etc.) is a common computer science solution to speeding things up
by otherwise eliminating a computationally-costly process.

Finally, now that we have everything, let's run ``tview``.

::

    samtools tview combined_vs_YsGenome_bwa.sorted.bam genome.fa

You'll be whisked away to a sequence map. `This
tutorial <http://biobits.org/samtools_primer.html#VisualizingReads>`__
has a guide to navigating. Use ``h`` and ``l`` to move right and left;
``j`` and ``k`` for up and down. (Arrow keys seem to work too.)

Type ``?`` for help. You may need to expand your terminal window to see
all the way to the bottom of the ``help`` menu.

Hit ``q`` to exit.

Let's go somewhere specific.

::

    samtools tview -p chrII:278586 combined_vs_YsGenome_bwa.sorted.bam genome.fa

You can also go to a location when ``tview`` by typing ``g`` for
``goto`` and entering the location. Try with ``chrXII:289738``. (You may
need to expand your terminal window to see all the way to the bottom.)

Peak Predictions with MACS
--------------------------

We'll use popular program MACS for peak prediction. The current version
is 2. and Tao Liu maintains it
`here <https://github.com/taoliu/MACS/>`__. We'll use the sub-command
``callpeak``, this being the main function of MACS. See
`here <https://github.com/taoliu/MACS/>`__ for a guide to the options.

::

    macs2 callpeak -t combined_vs_YsGenome_bwa.bam --name gal4 --gsize 1.21e7 --nomodel --shift -100 --extsize 200

``--gsize`` is the genome size option and needs to be set or it will use
human as default.

``name`` is to give the output files identifying names.

Finally, ``--nomodel --shift -100 --extsize 200`` comes from `the
documentation on Github concerning the shift
option <https://github.com/taoliu/MACS/#--shift>`__. These are suggested
for DNAse-Seq datasets and so used here since ChIP-exo shares some
similarities.

I found same result with or without the use of the tag size option,
``--tsize 35``, and so I left it out.

Note that MACS originally required the ChIP data in BED format so at
lest we dodged **ONE** conversion.

Additionally, ``-outdir`` is a great option to use to help you keep
organized. We are purposefully not using it today only to make things
easy for navigating on these transient machines. Obviously, this isn't
good data management practice.

Peaks relative to known genomic features with CEAS
--------------------------------------------------

We now go onto visualizing the ChIP enrichment signals relative known
genomic features. For that we'll use
`CEAS <http://liulab.dfci.harvard.edu/CEAS/>`__, another Python package
by Hyunjin Shin and Tao Liu from when they were in Xiaole Shirley Liu’s
Lab. (There is `a web-server
version <http://ceas.cbi.pku.edu.cn/submit.htm>`__ but it is limited to
mouse and human and seems no longer operating.)

Probably not surprisingly at this point, running CEAS requires some
additional preparation. CEAS requires the peaks to be specified in the
BED format, whereas the signal is to presented in a WIG file. We have
the peaks in the Bed format already. We need to convert the signal from
a sorted Bam file to WIG.

Additionally, the CEAS program operates on a SQLITE database that
contains the information on the annotated regions of the genome. We need
to build that database ourselves since there is not a prebuilt one for
yeast available from the author's site. If we provide as option '``d``
the name of the genome assembly and as option ``g`` the annotation
table, both as the are referred to at `the UCSC Genome Bioinformatics
site <http://genome.ucsc.edu/cgi-bin/hgTables?hgsid=424110275_bAsBLMcKlHRh22YW9Ta07ZJIeAJa&clade=other&org=0&db=0&hgta_group=genes&hgta_track=knownGene&hgta_table=knownGene&hgta_regionType=genome&position=&hgta_outputType=primaryTable&hgta_outFileName=>`__,
a utility included in the CEAS installation package will contact the
`the UCSC Genome Bioinformatics
site <http://genome.ucsc.edu/cgi-bin/hgTables?hgsid=424110275_bAsBLMcKlHRh22YW9Ta07ZJIeAJa&clade=other&org=0&db=0&hgta_group=genes&hgta_track=knownGene&hgta_table=knownGene&hgta_regionType=genome&position=&hgta_outputType=primaryTable&hgta_outFileName=>`__
and build the database for us. In addition to indicating the genome and
annotation table, the annotation building process requires a wiggle file
that describes the genomic locations that are valid for data selection.
For example, a tiling array may only cover some parts of a genome. In
our case, we theoretically should have covered all the genome and so we
simply generate a wiggle file that covers the full length of each and
every chromosome. In the interest of time, this has already been done
for you; the file is ``sacCer3.wig``. You can acquire it by running the
command below on the command line of your instance or `get it at github
if you are running this pipeline
locally <https://raw.githubusercontent.com/fomightez/may2015feng_gr_m/master/sacCer3.wig>`__;
information about making this file is
`here <http://fenglabwkshopmay2015.readthedocs.org/en/latest/Making%20Wiggle%20file%20Covering%20Genome/>`__.

::

    wget https://raw.githubusercontent.com/fomightez/may2015feng_gr_m/master/sacCer3.wig

To build the annotation database run

::

    build_genomeBG -d sacCer3 -g sgdGene -w sacCer3.wig -o sc3.db

(NOTE for anyone running this not on Amazon EC2 instances: the above
command needs to access an external MySQL database and I believe may be
a problem on certain networks.)

We now have a custom database ``sc3.db`` that can be used to generate
reports with CEAS.

What file formats do we need for running CEAS?

`CEAS manual <http://liulab.dfci.harvard.edu/CEAS/usermanual.html>`__

We still need a WIG file with the Chip-Seq data. Following from the
advice of Stew on `this page
Bam2Wig <https://www.biostars.org/p/2699/>`__ and `this
page <https://www.biostars.org/p/10569/>`__, we can use the sorted bam
file, ``combined_vs_YsGenome_bwa.sorted.bam``, to make such a file.
(Yes, this file contains the same information as BAM file used for peak
predictions with MACS2, but even related bioinformatics software will
often require files in different formats.)

::

    samtools mpileup combined_vs_YsGenome_bwa.sorted.bam | perl -ne 'BEGIN{print "track type=wiggle_0 name=combined_vs_YsGenome_bwa description=combined_vs_YsGenome_bwa\n"};($c, $start, undef, $depth) = split; if ($c ne $lastC) { print "variableStep chrom=$c\n"; };$lastC=$c;next unless $. % 10 ==0;print "$start\t$depth\n" unless $depth<3;'  > combined_vs_YsGenome_bwa.wig

Finally, CEAS uses the peaks gal4\_summits.bed predicted with MACS, the
wiggle file combined\_vs\_YsGenome\_bwa.wig with the ChIP-Seq signal and
the custom built database sgd.db:

::

    ceas -g sc3.db -b gal4_summits.bed -w combined_vs_YsGenome_bwa.wig

The reports are generated as the file ``gal4_summits.pdf`` and contains
several graphs and plots.

How do you view the pdf? The issue here is that it is on a EC2 instance
at Amazon and not your local computer. While `it is fairly
straightforward to put it on your computer (I highly recommend
scp) <http://fenglabwkshopmay2015.readthedocs.org/en/latest/downloading_and_uploadingEC2/>`__,
in the interest of time I have placed it in the associated Github
repository. Click
`here <https://github.com/fomightez/may2015feng_gr_m/blob/master/gal4_summits.pdf>`__
or paste the link below into your browser URL bar.

::

    https://github.com/fomightez/may2015feng_gr_m/blob/master/gal4_summits.pdf

See the legends at the bottom of
`here <http://liulab.dfci.harvard.edu/CEAS/usermanual.html>`__ for help
in interpreting the graphs.

The first plot shows 5.9% mappable on genome background because we
didn't provide a typical control sample here and it just calculates 5.9%
without such input due to the way the calculation works having a cutoff
of 5.9. See `in the CEAS
paper <http://www.ncbi.nlm.nih.gov/pmc/articles/PMC1538818/>`__

    where the background is the 9th order nucleotide Markov dependency
    estimated from the human genomic sequence. A score cutoff of Max
    (5,0.9 × Motif Relative Entropy) is used to call a motif a hit.

Plus it is probably moot for the ChIP-exo data used here. Recall from
Rhee and Pugh, 2011

    Uncrosslinked nonspecific DNA is largely eliminated by exonuclease
    treatment, as evidenced by the repeated failure to generate a
    ChIP-exo library from a negative control BY4741 strain. Therefore,
    use of an exonuclease makes comparisons to input DNA or mock IP
    moot, in that such DNA is destroyed.

Optional: Look at reads and/or peaks in IGB or IGV or SeqMonk OR UCSC genome browser
------------------------------------------------------------------------------------

In most viewers the MACS2 output can be directly loaded into according
to documentation. Your mileage may vary. We will not be doing this in
the interest of time today.

Comparison to published data
----------------------------

Preparation
^^^^^^^^^^^

Conveniently, the authors and SGD made the locations they deemed binding
sites available as a BED file.

You can use your browser on your local computer to get it at

::

    http://downloads.yeastgenome.org/published_datasets/Rhee_2011_PMID_22153082/track_files/Rhee_2011_Gal4_ChIP_exo_bound_locations_V64.bed

Alternatively if you wish to stay working on the command line...

Downloading on a Mac

::

    curl -o Rhee_2011_Gal4_ChIP_exo_bound_locations_V64.bed "http://downloads.yeastgenome.org/published_datasets/Rhee_2011_PMID_22153082/track_files/Rhee_2011_Gal4_ChIP_exo_bound_locations_V64.bed"

Downloading on a Linux instance:

::

    wget -O Rhee_2011_Gal4_ChIP_exo_bound_locations_V64.bed "http://downloads.yeastgenome.org/published_datasets/Rhee_2011_PMID_22153082/track_files/Rhee_2011_Gal4_ChIP_exo_bound_locations_V64.bed"

Now we could compare that to our peak summits.

Before we do that, however, let's do a little more filtering based on
standards set in the paper.

From the supplemental information ``Data Analysis`` section for the
paper Rhee and Pugh, 2011: From ``Set of Gal4 bound locations``

    Set of Gal4 bound locations. The final set of 15 Gal4 bound
    locations was determined to be those locations having all of the
    following properties: 1) peak-pair distances between -1 and 61 bp,
    2) peak-pair found in ≥2 of 3 replicates, in which a replicate
    peak-pair was <28 bp away (midpoint to midpoint), 3) a median of ≥75
    normalized tags (see note above) per peak-pair (5% of the average
    tag count of all 15 peak-pairs), and 4) not at telomeric, tRNA, and
    rDNA regions.

We'll discuss some of this shortly, but for now let's focus on property
four. The main issue I saw in my development of this workshop was
summits in the telomeric DNA regions and so let's at least get rid of
those. Using the lengths of the chromosomes that is at the beginning of
several of our SAM file, it would be fairly easy to make a Python
program to pick a distance from the ends of the chromosomes and remove
any summits in those. However, there is a way to filter out regions that
is more general and we'll use that next.

Filter additional regions
^^^^^^^^^^^^^^^^^^^^^^^^^

Ideally we would filter on all the regions, the authors of the study
also filtered on. In the interest of time though, today we'll just focus
on one. The telomeric sites.

The Rhee and Pugh, 2011 paper used genomic features to filter out
results from peak-calling. We want to do this as well.

A good, general approach seems:

-  get list or lists as Bed file from YeastMine

-  Uses Bedtools with ``intersectBed -v`` to filter out those summits
   that are in those regions. Our example here will be telomeres.

The steps for getting the telomere ``Bed`` file are spelled out
`here <http://fenglabwkshopmay2015.readthedocs.org/en/latest/telomere_bed/>`__.

I'll demonstrate the process of acquiring the file fairly fast here, and
then we'll just download the result file in the interest of time. The
page at the link above spells the approaches if you are doing this
yourself.

::

    wget https://raw.githubusercontent.com/fomightez/may2015feng_gr_m/master/telomere_regions.bed

Now to filter with that file. We'll use `bedtools's intersect
function <http://bedtools.readthedocs.org/en/latest/content/tools/intersect.html?highlight=intersect>`__
with the ``-v`` option to exclude those that have any overlap with the
telomere regions.

::

    bedtools intersect -v -a gal4_summits.bed -b telomere_regions.bed > filtered_gal4_summits.bed

Now we are ready to compare.

Compare
~~~~~~~

Compare your ``filtered_gal4_summits.bed`` to
``Rhee_2011_Gal4_ChIP_exo_bound_locations_V64.bed``.

I am going to write the command line commands but feel free to use
whatever method you want to examine and compare the files.

::

    head -30 filtered_gal4_summits.bed

    tail -15 Rhee_2011_Gal4_ChIP_exo_bound_locations_V64.bed

Interesting. While not perfect our summits do overlap with many of
theirs.

Two parts from the supplemental information ``Data Analysis`` section
for the paper Rhee and Pugh, 2011 deserve highlighting in the light of
this:

From ``Alignment to genome, peak calling, and data sharing``

    The resulting sequence read distribution was used to identify peaks
    on the forward (W) and reverse (C) strand separately using the peak
    calling algorithm....

From ``Set of Gal4 bound locations``

    Set of Gal4 bound locations. The final set of 15 Gal4 bound
    locations was determined to be those locations having all of the
    following properties: 1) peak-pair distances between -1 and 61 bp,
    2) peak-pair found in ≥2 of 3 replicates, in which a replicate
    peak-pair was <28 bp away (midpoint to midpoint), 3) a median of ≥75
    normalized tags (see note above) per peak-pair (5% of the average
    tag count of all 15 peak-pairs), and 4) not at telomeric, tRNA, and
    rDNA regions.

Note two huge differences probably contribute largely to the differences
in our results:

::

    * Largely in the interest of time, I didn't have us do the peak calling with the forward and reverse mapped reads separately and then process those peaks further as the authors describe. Since the authors went on to use the peak-pair midpoint to locate the binding site, as described in Figure S3, I figured the peak calling algorithm would largely average this out to locate the midpoint on its own and that should be good enough for our purposes today.

    * As best I can tell, we were only supplied data from 1 replicate. Their properties also mention things about 3 replicates but they only posted one replicate for Gal4 data it seems. I checked all 5 sets of data linked and only see the one replicate for Gal4 mentioned. This will obviously factor into differences with the results we get in workshop as compared to the paper.

    * Additional differences could arise from the different software used in the pipeline and the more limited filtering we performed. For example, the MACS 2012 guide suggests using more specialized tools for data types like DNase-seq and ChIP-exo is similar to that.

Motif discovery with MEME
-------------------------

Preparation
^^^^^^^^^^^

To look for motifs, let's try to take double the read length (35 \* 2)
of flanking sequence on each side of the peaks to account for reads
going either direction from the peak summits and allowing ample space
for a reasonable size DNA binding site. We'll use ``bedtools slop`` from
the Bedtools collection to expand the ranges upstream and downstream to
the locations of our peaks.

::

    bedtools slop -i filtered_gal4_summits.bed -g genome.fa.fai -b 70 > gal4_summits_more.bed

If you'd like to use the Rhee and Pugh 2011 data, the command is

::

    bedtools slop -i Rhee_2011_Gal4_ChIP_exo_bound_locations_V64.bed -g genome.fa.fai -b 29 > rhee_more.bed

Note: the ``genome.fa.fai`` file has summarized information about the
lengths of each chromosome that ``bedtools slop`` can use to limit the
ranges added to intervals to not extend beyond the actual length of the
chromosome sequences. (The support for .fai files for ``bedtools slop``
was only noted among the comments at the bottom of the page
`here <http://bedtools.readthedocs.org/en/latest/content/tools/slop.html?highlight=slop>`__.)

(The 29 base selection for the Rhee and Pugh Bed file was simply an
arbitrary value based around the midpoint of the peak-pair distances
identified in the paper. Obviously if we were doing this for actual
discovery we may have wanted to try a few different values here, or
better yet given the method, worked out for ourselves the peak-pair
values for our data.)

The ``Bed`` file just listed start and end locations for the peaks and
we simply expanded this window with ``bedtools slop``. We actually need
the sequences specified by our expanded ranges in order to provide the
MEME analysis tool with the sequences as input. We'll use another tool
from the Bedtools collection to accomplish this.

I am going to illustrate the commands for both our data and the
published data here for the next few commands. You can see how our data
comes out today using the MEME tool and then check back and see how it
would come out with there data if you'd like.

::

    bedtools getfasta -fi genome.fa -bed gal4_summits_more.bed -fo gal4_summits_more.fa

If you'd like to use the Rhee and Pugh 2011 data, the command is

::

    bedtools getfasta -fi genome.fa -bed rhee_more.bed -fo rhee_more.fa

Expand your window in which you are working as tall as you can and then
enter on command line

::

    less gal4_summits_more.fa

If you'd like to use the Rhee and Pugh 2011 data, the command is

::

    less rhee_more.fa

Now highlight in your terminal and copy the highlighted text to the
clipboard.

Open your text editor.

Paste the text you copied into a text editor program and save the file
as ``gal4_summits_more.fa`` or ``rhee_more.fa``, respectively.

Now using the browser on your computer go to
`here <http://meme.nbcr.net/meme/tools/meme>`__ . Use the above file to
upload it where it asks for ``Input the primary sequences``. (Supposedly
`here <http://meme-suite.org/tools/meme>`__ is the most up-to-date
version of the site. However, the Upstate network said it was
unavailable or it violated policy and has been blocked when I submitted
jobs there.)

Under ``Select the number of motifs``, adjust the number of motifs to
find to one.

Provide an e-mail address.

Click on 'Start Search' to submit your task. It will be a short wait.
Your results will be available in your browser soon. No need to check
email most likely.

Click on ``MEME html output`` and view the results by hitting blue arrow
below the ``More`` column on the next screen.

How do your results match up with `the figure 3 MEME output logo from
Rhee and Pugh,
2011 <http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3243364/figure/F3/>`__?

How do these results match up with `the structural basis of DNA binding
by
Gal4p <http://proteopedia.org/wiki/index.php/User:Wayne_Decatur/Gal_4>`__?

`The slides <http://fomightez.github.io/MayFourteenth_slides>`__ will be
used to discuss the above two questions in more depth.

Why not PRECISELY same (even when using their file)? We have to imagine
we were coming at this naively and trying to identify a motif. (Or least
be performing a proof of concept experiment as they were here.) This was
obviously just an initial first pass to see if any motif is identified.
One is. This would obvious warrant further examination. In this case,
we'd also have to look and see that because of the amount of flanking
sequence added to each site region, some of the sites identified in the
ChIP-exo data overlap with neighboring sites in the sequences submitted
to MEME or are left out because the peak identification in MACS2 we used
seems to squelch out neighboring sites. Thus we have probably biased
this rough first pass to the *best* ones when faced with overlap. Today
was just meant to give you a flavor for the steps involved if you were
trying to use this to search for a novel motif. In a thorough
investigation of the motif there'd be subsequent steps and the results
seen on the left side of `Figure 3 of Rhee and Pugh,
2011 <http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3243364/figure/F3/>`__
are likely the result of such further analysis. The authors may have
restricted the sites they submitted down to say the best match within
the distances they had identified with the peak pairs. Additionally,
judging by the indication to the orientation of the gene, I think they
may have used the advanced option settings in MEME to limit the search
to given strand and provided the upstream sequence from the gene which
is not what we did here. Additionally, as touched in numerous places,
their approach to identify peak pairs and sites was more detailed than
what we did here, and had additional replicates and filtering based on
features (tRNA and rDNA) to confirm or rule out peaks, and so the
upstream results before MEME were obviously different.

(Actually, I have used the data available in `the Rhee and Pugh Gal4 Bed
file available from
SGD <http://downloads.yeastgenome.org/published_datasets/Rhee_2011_PMID_22153082/track_files/Rhee_2011_Gal4_ChIP_exo_bound_locations_V64.bed>`__
to eliminate the overlaps and found I still don't get an exact match to
the `Figure 3 MEME output
logo <http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3243364/figure/F3/>`__.
It is *very, very* close. However, there is a further complication.
`Figure
3 <http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3243364/figure/F3/>`__
shows the sequence of 15 binding sites. If you count the lines of the
actual data in the Gal4 Bed file, you'll see there is only 14. Likewise,
the Gal4 data in the supplemental Excel file with the publication has a
heading of *15 Gal4 binding sites*, but there are only 14 there. One of
the Gal7 sites shown in Figure 3 is absent in the Excel spreadsheet, and
hence the Bed file because the Bed file header comments says the SGD
curators used supplemental data to make the Bed file. Even adjusting for
that and the orientation, plus setting the motif to have to be exactly
19 bps long, I didn't obtain exactly their motif logo, and so maybe due
to a difference in other setting(s) or different software version.)

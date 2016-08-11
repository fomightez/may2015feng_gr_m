.. contents::
   :depth: 3.0
..

Appendix: Setting up a Mac for the workshop
===========================================

This was current Spring 2015 and done on a Mac running Mavericks. Keep
in mind things may change subsequently.

Caveat: I found two steps not to work on my work iMac and I strongly
suspect at least one of those cases is blocked by the Upstate's network.

These are my notes concerning installing many of the bioinformatic
software packages on a Mac. This is basically my own, Mac-based
counterpart to `the ANGUS listing of software packages to install on
Linux machines from
2013 <http://ged.msu.edu/angus/2013-04-assembly-workshop/installing-software.html>`__.
Some parts are more detailed than others. Generally those that were more
difficult than a normal Mac installation where you download in your
browser and click on it in the finder are described.

General software from previous installations prior to 2015
----------------------------------------------------------

Many of these adapted from
http://ged.msu.edu/angus/tutorials-2012/bwa\_tutorial.html, updating
links as I went.

-  `BWA (Burrows-Wheeler Aligner) <http://bio-bwa.sourceforge.net/>`__
-  Bowtie2
-  `SAMtools <http://www.htslib.org/>`__ OLD site:
   http://samtools.sourceforge.net/
-  `FastQC <http://www.bioinformatics.babraham.ac.uk/projects/download.html#fastqc>`__
-  IGB
-  IGV

Added specifically for developing ChIP-seq workshop pipeline April 2015:
------------------------------------------------------------------------

-  `FASTX-Toolkit <http://hannonlab.cshl.edu/fastx_toolkit/download.html>`__
-  `SRA
   Toolkit <http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software>`__
-  `libgtextutils <http://hannonlab.cshl.edu/fastx_toolkit/download.html>`__
-  `MACS2 <https://pypi.python.org/pypi/MACS2>`__ and
   `here <https://github.com/taoliu/MACS/>`__
-  `CEAS <http://liulab.dfci.harvard.edu/CEAS/download.html>`__
-  `BEDtools <http://bedtools.readthedocs.org/en/latest/content/installation.html>`__

Installing notes for prior installations
----------------------------------------

Mainly the prior ones were from yeast analyses on work iMac

BWA
~~~

Waynes-iMac:~ Wayne$ mkdir bioinf Waynes-iMac:~ Wayne$ cd bioinf

next line updated from http://sourceforge.net/projects/bio-bwa/files/

::

    curl -O -L http://sourceforge.net/projects/bio-bwa/files/bwa-0.7.5a.tar.bz2
    tar xvfj bwa-0.7.5a.tar.bz2
    cd bwa-0.7.5a
    make
    sudo cp bwa /usr/local/bin

IT WILL ASK FOR PASSWORD HERE

::

    cd ../

FastQC
~~~~~~

According to
http://www.bioinformatics.babraham.ac.uk/projects/fastqc/INSTALL.txt, I
wanted the zipped file of FastQC to be able to run it on command line,
EVEN for Mac OS. Note that the Mac OS GUI version (from '.dmg' download)
does load even gzipped fastq files and the report can be saved to give
the same thing the command line does and so you can do it via a more
tpyical installtion and run it not on the command line if you'd prefer
for a Mac; I don't know about Linux GUI options for this program for
installing and running on local machines. So I downloaded it, unzipped,
and now I need to give it permissions to run as exectuable form command
line, following
http://ged.msu.edu/angus/tutorials-2012/fastqc\_tutorial.html:

::

    cd ../
    cd Downloads/
    cd FastQC/
    chmod +x fastqc

Note that the GUI version (from '.dmg' download) does load even gzipped
fastq files and the report can be saved to give the same thing the
command line does.

IGV and IGB
~~~~~~~~~~~

Downloaded IGB (intergrated genome browser) Downloaded IGV (intergrated
genome viewer)

SAMtools and Bowtie2
~~~~~~~~~~~~~~~~~~~~

So I started reviewing how to download:
http://sourceforge.net/projects/samtools/files/samtools/0.1.19/samtools-0.1.19.tar.bz2
(I note on
http://ged.msu.edu/angus/2013-04-assembly-workshop/installing-software.html
that in April 2013, Titus was getting Samtools from github.) Looking
ahead, I did the same for bowtie. Seems bowtie is at
http://bowtie-bio.sourceforge.net/index.shtml and bowtie2 is out that is
supposed to be better for long reads, i.e., those over 50 bp. Since the
ones from the mitochondrial landscape paper are over 50, maybe I should
try and get bowtie2 working. See -->
http://bowtie-bio.sourceforge.net/bowtie2/index.shtml. Download from
http://downloads.sourceforge.net/project/bowtie-bio/bowtie2/2.1.0/bowtie2-2.1.0-macos-x86\_64.zip
. In april 2013, Titus was installing both
http://ged.msu.edu/angus/2013-04-assembly-workshop/installing-software.html.
Installed these two:

::

    cd samtools-0.1.19
    make
    sudo cp samtools /usr/local/bin
    cd ../
    cd bowtie2-2.1.0

[MAKE FAILED BUT LOOKS LIKE SINCE I DOWNLOADED MAC SPECIFIC ONE, THAT IT
IS ALL SET. JUST NEED TO COPY EXECUTABLE TO PATH]

::

    sudo cp bowtie2 /usr/local/bin

Howerver, executing it gave an error

::

    Error: Expected bowtie2 to be in same directory with bowtie2-align:

Searching that error, I found
http://seqanswers.com/forums/showthread.php?t=29727 which lead to
http://bowtie-bio.sourceforge.net/bowtie2/manual.shtml#adding-to-path
that said to add all the executables to your path " make sure that you
copy all the executables, including bowtie2, bowtie2-align,
bowtie2-build and bowtie2-inspect." So finsished by,

::

    sudo cp bowtie2-align /usr/local/bin
    sudo cp bowtie2-build  /usr/local/bin
    sudo cp bowtie2-inspect /usr/local/bin

NOW IT WORKS WHEN I TYPE 'bowtie2'

More recent installations
-------------------------

Added specifically for ChIP-seq pipeline May 2015

FastX Toolkit
-------------

Download
`FASTX-Toolkit <http://hannonlab.cshl.edu/fastx_toolkit/download.html>`__
from http://hannonlab.cshl.edu/fastx\_toolkit/download.html Also
downloaded
`libgtextutils-0.7.tar.gz <http://hannonlab.cshl.edu/fastx_toolkit/download.html>`__
from there as well. THIS ONE NEEDS TO BE INSTALLED FIRST. (See `Software
packages to
install <http://ged.msu.edu/angus/2013-04-assembly-workshop/installing-software.html>`__
and `Installing FastX on
Mac <http://www.lincw.com/2014/01/installing-fastx-on-mac/>`__ for help
on Linux and Macs, respectively. Note on the Mac, I always seem to need
to change the from the ``./configure && make && make install`` Titus
uses in Unix/Linux to ``./configure && make && sudo make install`` for
the Mac. I did the first four lines of the Mac approaches by hand and I
was having trouble getting the Linux commands working on the Mac because
I kept getting errors intalling libgtextutils, which lead to errors
installing pkg-config, but finally got both to work (see detailed notes
and steps below). )

Unzipped and moved to my ``bioinf`` folder.

Items to note about the next steps:

-  libgtextutils NEEDS TO BE INSTALLED FIRST!! The FASTX-Toolkit relies
   on this and seems to look for related items during installation.

-  You have to install ``pkg-config`` and edit your PATH using the text
   on lines 6 and 7 in part II of the instructions `Installing FastX on
   Mac <http://www.lincw.com/2014/01/installing-fastx-on-mac/>`__\ involving
   setting the path (see page 87 of Haddock and Dunne's Practical
   Computing for Biologists; although doing it a different way here!!!)
   AS ONE LINE ON COMMAND LINE or you'll get this error below:

   checking for GTEXTUTILS... configure: error: in
   \`/Users/Wayne/bioinf/fastx\_toolkit-0.0.14': configure: error: The
   pkg-config script could not be found or is too old. Make sure it is
   in your PATH or set the PKG\_CONFIG environment variable to the full
   path to pkg-config.

   Alternatively, you may set the environment variables
   GTEXTUTILS\_CFLAGS and GTEXTUTILS\_LIBS to avoid the need to call
   pkg-config. See the pkg-config man page for more details.

   To get pkg-config, see http://pkg-config.freedesktop.org/. See
   \`config.log' for more details Waynes-iMac:fastx\_toolkit-0.0.14
   Wayne$ make make: \*\*\* No targets specified and no makefile found.
   Stop. Waynes-iMac:fastx\_toolkit-0.0.14 Wayne$

In part ``2`` of `Installing FastX on
Mac <http://www.lincw.com/2014/01/installing-fastx-on-mac/>`__, lines
six and seven which need to be combined into one command to read:

::

    export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH

-  According to
   `here <http://www.fantageek.com/318/install-pkg-config-for-mac-osx/>`__
   and
   `here <https://cduu.wordpress.com/2011/11/26/install-pkg-config-on-mac-os-x/>`__
   `MacPorts <https://www.macports.org/install.php>`__ is supposed to
   make installing ``pkg-config`` easy. However, after going through the
   long version of installing MacPorts, which needs the Apple developer
   tools ( to `install Xcode and the developers
   tools <http://guide.macports.org/#installing.xcode>`__ or `install
   developers tools on your
   Mac <http://osxdaily.com/2014/02/12/install-command-line-tools-mac-os-x/>`__
   ). I couldn't get it to work even carefully following `these
   instructuons <https://cduu.wordpress.com/2011/11/26/install-pkg-config-on-mac-os-x/>`__.
   But I noticed there was an 'install' document in the ``pkg-config``
   folder I downloaded and unzipped from
   `here <http://pkgconfig.freedesktop.org/releases/>`__ and it
   mnetionned the usual ``./configure && make && make install`` steps so
   I tried ``./configure && make && sudo make install``. Seemed to work
   but hit an error about 'glib' but in the error report it said I could
   tell it to use internal glib with an option and that worked. The
   exact text was
   ``or pass --with-internal-glib to configure to use the bundled copy``.

The actual, worked out additional steps for installing the FASTX-Toolkit
on Mac OS X (Mavericks, presently).

::

    Download the latest version of pkg-config from http://pkgconfig.freedesktop.org/releases/
    Unzip it.
    Open terminal, navigate to the folder you unpacked, and issue the command:

        ./configure --with-internal-glib && make && sudo make install

    Now you are finally ready to finish the steps for libgtextutils and fastx_toolkit installing
    Navigate to the directory with `libgtextutils`. Here is the record of what I did. You want to run everything after the $ in your own terminal.
        Waynes-iMac:bioinf Wayne$ cd libgtextutils-0.7/
        Waynes-iMac:libgtextutils-0.7 Wayne$ ./configure && make && sudo make install
        Waynes-iMac:libgtextutils-0.7 Wayne$ cd ../
        Waynes-iMac:bioinf Wayne$ cd fastx_toolkit-0.0.14/
        Waynes-iMac:fastx_toolkit-0.0.14 Wayne$ export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH
        Waynes-iMac:fastx_toolkit-0.0.14 Wayne$ ./configure && make && sudo make install

SRA Toolkit
-----------

`SRA toolkit
downloading <http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software>`__

Home SRA toolkit installation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Downloaded Mac version from
`here <http://www.ncbi.nlm.nih.gov/Traces/sra/?view=software>`__ and
unpacked in finder.

Then followed `SRA Toolkit Installation and Configuration
Guide <http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=std>`__
exactly.

Specifically, I made a folder in my ``schmmitlabwayne`` user folder that
I named ``sra-toolkit``.

I dragged all the files in the ``/bin`` folder in the unpacked
``sratoolkit.2.4.5-2-mac64`` folder into that folder I jsut named
``sra-toolkit``. Ran initial command to see if I was on rigth track

::

    ~/sra-toolkit/fastq-dump

It yielded the ``fastq-dump`` manual page. Sofar it is looking
promising. (But I got that far on work computer.)

Next, as a full test I issued the command below in the terminal

::

     ~/sra-toolkit/fastq-dump -X 5 -Z SRR390728

This worked. Output:

::

    Read 5 spots for SRR390728
    Written 5 spots for SRR390728
    @SRR390728.1 1 length=72
    CATTCTTCACGTAGTTCTCGAGCCTTGGTTTTCAGCGATGGAGAATGACTTTGACAAGCTGAGAGAAGNTNC
    +SRR390728.1 1 length=72
    ;;;;;;;;;;;;;;;;;;;;;;;;;;;9;;665142;;;;;;;;;;;;;;;;;;;;;;;;;;;;;96&&&&(
    @SRR390728.2 2 length=72
    AAGTAGGTCTCGTCTGTGTTTTCTACGAGCTTGTGTTCCAGCTGACCCACTCCCTGGGTGGGGGGACTGGGT
    +SRR390728.2 2 length=72
    ;;;;;;;;;;;;;;;;;4;;;;3;393.1+4&&5&&;;;;;;;;;;;;;;;;;;;;;<9;<;;;;;464262
    @SRR390728.3 3 length=72
    CCAGCCTGGCCAACAGAGTGTTACCCCGTTTTTACTTATTTATTATTATTATTTTGAGACAGAGCATTGGTC
    +SRR390728.3 3 length=72
    -;;;8;;;;;;;,*;;';-4,44;,:&,1,4'./&19;;;;;;669;;99;;;;;-;3;2;0;+;7442&2/
    @SRR390728.4 4 length=72
    ATAAAATCAGGGGTGTTGGAGATGGGATGCCTATTTCTGCACACCTTGGCCTCCCAAATTGCTGGGATTACA
    +SRR390728.4 4 length=72
    1;;;;;;,;;4;3;38;8%&,,;)*;1;;,)/%4+,;1;;);;;;;;;4;(;1;;;;24;;;;41-444//0
    @SRR390728.5 5 length=72
    TTAAGAAATTTTTGCTCAAACCATGCCCTAAAGGGTTCTGTAATAAATAGGGCTGGGAAAACTGGCAAGCCA
    +SRR390728.5 5 length=72
    ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;9445552;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;446662

Success! See
``terminal console accompanying facile installation of sra toolkit on home Mac``
for the actual few commands run on terminal and output.

Problematic effort on Mac at work
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

So far installation on computer at work has not resulted in an SRA
toolkit able to output a fasta formatted file. It gets hung up with the
SRA file in the cache.

MACS2
-----

When I search ``macs2`` I found it at https://pypi.python.org/pypi/MACS2
. The site being ``pypi.python.org`` indicated to me that I should be
able to use the package manager ``pip`` on my Mac to easily download and
install.

That was the case, I simply typed in my Mac's terminal:

::

    pip install macs2

Sanity check for verifying installation

::

    macs2 -h

Display usage information so all is good.

CEAS
----

Downloaded `here <http://liulab.dfci.harvard.edu/CEAS/download.html>`__.

Unpacked in Finder and moved contents of unpacked folder to a newly
created folder called ``CEAS`` that I made within my standard
bioinformatics working directory.

In terminal navigated to that folder where unpacked and ran

::

     sudo python setup.py install

It unpacked. Writing
/Users/Wayne/Library/Enthought/Canopy\_64bit/User/lib/python2.7/site-packages/CEAS\_Package-1.0.2-py2.7.egg-info

Sanity check. Typed in terminal

::

    ceas

Reported back about program and options and so it works.

CEAS's ``build_genomeBG`` utility needs to access external databases so
in an attempt to get it to work on work iMac, I tried
``pip install MySQL-Python`` after reading a few places how to install
it.

No luck so far. When trying to do ``pip install MySQL-Python`` keep
getting

::

    EnvironmentError: mysql_config not found

AHA!! I at least got past the
``EnvironmentError: mysql_config not found`` problem, but I had to
follow ALL of the first part of user3429036's answer (at the top where
he assumed you tried all that) at
http://stackoverflow.com/questions/25459386/mac-os-x-environmenterror-mysql-config-not-found
. Below are those specific steps.

[I didn't touch Python because already installed.] So I had to first
install homebrew (so homebrew used to install mysql) in his approach.

::

    ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

Then I installed mysql which is where I think I was having problems. I
had not not done that before this.

::

    brew install mysql

Then I issued the command in the terminal

::

    export PATH=$PATH:/usr/local/mysql/bin

Finally I did

::

    pip install MySQL-Python

Progress. No more ``EnvironmentError: mysql_config not found``

But when I run ``build_genomeBG`` I get

::

    `_mysql_exceptions.OperationalError: (2003, "Can't connect to MySQL server on 'genome-mysql.cse.ucsc.edu' (60)")`

This is progress because I was getting

::

    /Users/Wayne/Library/Enthought/Canopy_64bit/User/lib/python2.7/site-packages/CEAS/inout.py:64: UserWarning: sqlite3 is used instead of MySQLdb because MySQLdb is not installed
      warnings.warn("sqlite3 is used instead of MySQLdb because MySQLdb is not installed")
    CRITICAL @ Mon, 27 Apr 2015 11:15:55: MySQLdb package needs to be installed to use UCSC or a local sqlite3 db file must exist.
    CRITICAL @ Mon, 27 Apr 2015 11:15:55: Check -g (--gdb). No such file or species as 'sgdGene'

But the ``build_genomeBG`` doesn't function. I don't think that is the
installation. \*\*\*\*\* Is it possible it is blocked by network at
work????? \*\*\*\*\*\* (Like I suspect SRA was?) Following from
http://genome.ucsc.edu/goldenpath/help/mysql.html lead me to the Google
groups which I searched. Lead me to ...

from
https://groups.google.com/a/soe.ucsc.edu/forum/#!searchin/genome/an$27t$20connect$20to$20MySQL$20server$20on$20$27genome-mysql.cse.ucsc.edu$27/genome/QlSWjaGaZq4/hLdo8hbcqHEJ
> If you're attempting to connect from an institution, it's possible
that your IT staff have external mysql connections blocked by default
for security reasons.

For touble shooting I tried on command line

::

    mysql --user=genome --host=genome-mysql.soe.ucsc.edu -A

Ughh!

::

    Waynes-iMac:bioinf Wayne$ mysql --user=genome --host=genome-mysql.soe.ucsc.edu -A
    ERROR 2003 (HY000): Can't connect to MySQL server on 'genome-mysql.soe.ucsc.edu' (60)
    Waynes-iMac:bioinf Wayne$

Seems we are blocked at Upstate????

Seems we are blocked at Upstate???? Consistent with my concern this is
the case and why this (and probably SRA toolkit) fail is that that
running ``build_genomeBG`` on a Ubuntu instance on AWS worked great.

::

    build_genomeBG -d sacCer3 -g sgdGene -w sacCer3.wig -o sc3.db

Command above on Amazon AWS resulted in generating ``sc3.db``. I
downloaded it from there to have on local Mac.

BEDtools
--------

Following
`here <http://bedtools.readthedocs.org/en/latest/content/installation.html>`__
at first it looked like this would be an easy installation. I recently
installed ``Homebrew`` on the Mac and it looks like I can just use that
to manage the installation.

::

    brew install bedtools

Unfortunately, despite what the nice looking installation instructions
seemed to indicate, that failed

::

    Waynes-iMac:bioinf Wayne$ brew install bedtools
    Error: No available formula for bedtools
    Searching formulae...
    Searching taps...
    homebrew/science/bedtools
    Waynes-iMac:bioinf Wayne$ bedtools
    -bash: bedtools: command not found
    Waynes-iMac:bioinf Wayne$ port install bedtools
    Error: Insufficient privileges to write to MacPorts install prefix.
    Waynes-iMac:bioinf Wayne$ sudo port install bedtools
    Password:
    Error: Port bedtools not found
    Waynes-iMac:bioinf Wayne$ sudo brew install bedtools
    Error: Cowardly refusing to `sudo brew install`
    You can use brew with sudo, but only if the brew executable is owned by root.
    However, this is both not recommended and completely unsupported so do so at
    your own risk.
    Waynes-iMac:bioinf Wayne$

Looking under the section `Compiling from source via Google
Code <http://bedtools.readthedocs.org/en/latest/content/installation.html>`__,
lead me to `the Google code
site <https://code.google.com/p/bedtools/>`__ to look up the version
number. That site said it was no longer to be hosted there as of end of
2103 and has `moved to Github <https://github.com/arq5x/bedtools2>`__.
Looking back, in fact the Github directions for installing are at the
bottom, although it is disfavored by saying it is the development
version.

Dowmloaded the zipped file from
`here <https://github.com/arq5x/bedtools2>`__ and unzipped it using
finder.

Then ran make in terminal.

::

     cd bedtools2-master
     make

Then finishing installation following method under `Compiling from
source via Google
Code <http://bedtools.readthedocs.org/en/latest/content/installation.html>`__
copied files to ``/usr/local/bin``.

::

    sudo cp ./bin/* /usr/local/bin

Sanity check. Typing

::

    bedtools

on command line gave usage information, and so it seems successfully
installed.

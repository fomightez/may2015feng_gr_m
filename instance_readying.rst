.. contents::
   :depth: 3.0
..

Appendix: Setting up an Amzon EC2 instance for the workshop
===========================================================

This was current Spring 2015. Keep in mind things may change
subsequently.

Building the Amazon EC2 instance
--------------------------------

You'll find how I set up these systems for the workshop below, which was
adapted from information mainly
`here <http://angus.readthedocs.org/en/2014/day1.html>`__,
`here <http://angus.readthedocs.org/en/2014/amazon/starting-up-a-custom-ami.html>`__,
and
`here <http://angus.readthedocs.org/en/2014/amazon/technical-guide.html>`__.

Keep in mind in an effort to make things go more smoothly today, I
eliminated a step that users on Windows operating system usually have to
perform. If you ever find yourself connecting to an Amazon EC2 instance
on Windows and need to generate a ppk file from your pem file, see
`here <http://angus.readthedocs.org/en/2014/amazon/log-in-with-ssh-win.html#generate-a-ppk-file-from-your-pem-file.>`__

record of software installation for NGS work on Amazon AWS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

These are my notes concerning installing many of the bioinformatic
software packages on a Linux AWS machines. This is basically my own
counterpart to `the ANGUS listing of software packages to install on
Linux machines from
2013 <http://ged.msu.edu/angus/2013-04-assembly-workshop/installing-software.html>`__
and `Technical guide to the 2014 ANGUS
course <http://angus.readthedocs.org/en/2014/amazon/technical-guide.html>`__.
Developed during preparation of machines for May 2015 workshop.

List of installed software
~~~~~~~~~~~~~~~~~~~~~~~~~~

-  basics like screen, git, curl, gcc comiler, python, and r, etc.
-  `BWA (Burrows-Wheeler Aligner) <http://bio-bwa.sourceforge.net/>`__
-  Bowtie2
-  `SAMtools <http://www.htslib.org/>`__ OLD site:
   http://samtools.sourceforge.net/
-  `FastQC <http://www.bioinformatics.babraham.ac.uk/projects/download.html#fastqc>`__
   <-- NOT IN ACTUAL LIST HERE YET **EDIT OUT FOR POSTING**??
-  `FASTX-Toolkit <http://hannonlab.cshl.edu/fastx_toolkit/download.html>`__
-  `SRA
   Toolkit <http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software>`__
-  `libgtextutils <http://hannonlab.cshl.edu/fastx_toolkit/download.html>`__
-  `MACS2 <https://pypi.python.org/pypi/MACS2>`__ and
   `here <https://github.com/taoliu/MACS/>`__
-  `BEDtools <http://bedtools.readthedocs.org/en/latest/content/installation.html>`__
-  `CEAS <http://liulab.dfci.harvard.edu/CEAS/download.html>`__

Machine preparation for April-May 2015
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Largely adapted from `Starting up a custom operating system guide for
ANGUS
course <http://angus.readthedocs.org/en/2014/amazon/starting-up-a-custom-ami.html>`__
and [Technical guide to the 2014 ANGUS
course](http://angus.readthedocs.org/en/2014/amazon/technical-guide.html

I think between 2013 and 2014, Titus moved the ANGUS course from using
the Starcluster AMIs to setting up the base Ubuntu system offered on
Amazon AWS, mainly relying on the
`apt-get <http://manpages.ubuntu.com/manpages/lucid/man8/apt-get.8.html>`__
package manager to facilitate this process.

Registering an instance
^^^^^^^^^^^^^^^^^^^^^^^

-  Logged into my Amazon AWS account.

-  In console, picked 'Key Pairs' from under ``Network & Security``.

   -  Created a new key pair so I can only use that for workshop. Called
      it ``workshop`` .
   -  Saved the .pem file to my ``Downloads`` directory on my Mac when
      prompted.\* Copied it to my folder for the workshop in Dropbox.

-  Followed the directions
   `here <http://angus.readthedocs.org/en/2014/amazon/starting-up-a-custom-ami.html>`__
   to set up ``Ubuntu Server 14.04 LTS (PV)`` using the ``workshop`` key
   I had just made.

   -  Note I tried to set up ``ami-7606d01e`` but searching it in
      ``community AMIs`` failed to yield anything.

   -  It is important to set up the (PV) version that is way down on the
      list of ``Quick Start`` choices and not the (HVM) version that is
      near the top of the list. Otherwise you won't have the option to
      select ``m1.xlarge``.

   -  I decided to go with ``m3.xlarge`` and not the ANGUS-recommneded
      ``m1.xlarge`` because since this will be a small dataset once
      demultiplexed I didn't have that many Gb to store on the machine
      and ``m3.xlarge`` had more compute units and is a little cheaper
      per hour. See `here <http://www.ec2instances.info/>`__ for the
      details of each.

   After (1) noticing I lost data in /mnt when I stopped and restarted
   (I thought /mnt was where we worked in Titus's class in the
   ANGUS-recommneded ``m1.xlarge``); and (2) reading this-->
   https://4sysops.com/archives/how-to-check-if-your-ec2-instance-uses-ssd/
   , I started thkinkg going with ``m3.xlarge`` was a bad idea because
   the drives are SSD and therefore only epehemerial. PLUS IT SEEMS I
   HAD TO SPECIFY I WANTED THEM ACCESSIBLE IN 'ADD STORAGE' SECTION?!?!
   I DO ONLY SEEM TO SEE ONE BECAUSE I SKIPPED THAT?? Given I skipped
   that in ``add storage`` and this Jason McCreary's answer at
   http://serverfault.com/questions/490597/wheres-my-ephemeral-storage-for-ec2-instance,
   I cannot understand why I even have access to one of them. (Maybe
   since those two things, Amazon fixed it so at least one of them there
   be default. YES!! When I making new ones I noticed the instance store
   0 one is listed by defult but you have to specifically add the next
   ones even if the price guide says you have them. You still have to
   actively enable any beyond the first one here.) Maybe I should try
   next with ``m1.xlarge`` and see if my data in /mnt survives stopping
   and restarting and if I can see all I was told I should have. I MADE
   A NEW INSTANCE. Under 'add storage' I added all four 'instance
   storage' 0 thru 3 I had listed as options. And then when I ran
   ``sudo fdisk -l`` in my intance I could see them all unlike before
   when I didn't add. However, when I do ``df -h`` I still only see one
   of the four. It seems one always defaults to /mnt even if I don't
   mount it and that is all I get unless I mount them?

   In light of this, I launched a brand new instance, with
   ``m3.xlarge``, but I set the root to 30 Gb on the step #
   ``Step 4: Add Storage`` step. On this one I plan to install the
   software and the experimental data and verify it has enough space and
   that it keeps the data when it is stopped and restarted.

   -  Choose ``us-east-1d`` for availabilty zone. (I meant to do this
      for the newest one but I missed it and defaulted to ``1a``.)

   -  selected my ``quicklaunch-0`` security group rules which has SSH,
      HTTP, and HTTPs included.

   -  Used the ``workshop`` key I had just made.

Logging in.
^^^^^^^^^^^

Follow
`here <http://angus.readthedocs.org/en/2014/amazon/log-in-with-ssh-mac.html>`__
if you are on a Mac or Linux local machine. If you computer is a Windows
PC, see
`here <http://angus.readthedocs.org/en/2014/amazon/log-in-with-ssh-win.html#logging-into-your-ec2-instance-with-putty>`__.
Note you have already been provided the key in ``.ppk`` form, and so you
want to skip to the ``Logging into your EC2 instance with Putty``
section. BELOW IS MY MAC INFO.

::

    * Protected my key on my computer from other users on this machine.

        chmod og-rwx workshop.pem

    * Used `ssh` to log on, replacing the `ec2-??-???-???-?.compute-1.amazonaws.com` network name part with the similar information from when you initiated your instance on the AWS console.

        ssh -i workshop.pem ubuntu@ec2-??-???-???-?.compute-1.amazonaws.com
        ssh -i workshop.pem ubuntu@ec2-50-19-16-34.compute-1.amazonaws.com

    * Now, once conncted, to log on as super user, I issued following two commands.

        sudo bash
        cd /root
        (use cd /usr/workshop when working on workshop analysis steps)

    The [first command](http://askubuntu.com/questions/57040/what-is-the-difference-between-su-sudo-bash-and-sudo-sh) restarts the bash shell with you using as the super user and the second sets you in the home directory of the super user.

    * To check out what we have you can type the command below to see

        df -h

    About half the `/dev/xvda1` is filled with the system and installed software. We'll soon add more and our data there. The `/mnt` directory amd is essentially the scratch space for our AWS EC2 instance. It will go away if the instance is stopped so we'll stay in `/dev/xvda1` so we don't have to keep adding our data in case we need to put the instance in `stop/pause` mode.

-  Exiting

To log out, type:

::

    exit
    logout

or just close the terminal or Putty window. (You cannot do this step
wrong because ultimately you (or me, for today) have control of the
instance in Amazon Web Services console.)

Preparing the instance for use
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Followed
`here <http://angus.readthedocs.org/en/2014/running-command-line-blast.html#updating-the-software-on-the-machine>`__
and MAINLY
`here <http://angus.readthedocs.org/en/2014/amazon/technical-guide.html>`__
to get started by putting on a lot of the basic software and some
special bioinifomatics ones.

::

    apt-get update
    apt-get -y install screen git curl gcc make g++ python-dev unzip \
            default-jre pkg-config libncurses5-dev r-base-core \
            r-cran-gplots python-matplotlib sysstat python-pip \
            ipython-notebook

(Oddly, second time I did this when setting up an instance with 30 Gb
storage in root, I had trouble [triggered an error about
``holding broken packages at one time`` when pasting the above command
all at once. I had to do line by line of the ``apt-get -y install``
command above. Then it worked fine. I recall the ANGUS course
documentation had watned about this command can be tricky to paste
right. I had edited my version some myself and maybe I disrupted
something about it?)

As described
`here <http://manpages.ubuntu.com/manpages/lucid/man8/apt-get.8.html>`__
the first command resynchronize the package index files from their
sources. The ``y`` option on the second line, the install command, says
to ``assume answering yes`` to any prompts and helps speed things up but
not needing the user to do anything.

Installed more specific software. Most is easy to install so I issued

::

    apt-get -y install samtools bedtools bwa fastx-toolkit python-mysqldb

    pip install macs2

The details of building this list is found below.

Installation notes for NGS software
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Many of these adapted from
http://ged.msu.edu/angus/tutorials-2012/bwa\_tutorial.html,
http://ged.msu.edu/angus/2013-04-assembly-workshop/installing-software.html,
and http://angus.readthedocs.org/en/2014/amazon/technical-guide.html,
updating as needed for May 2015 workshop.

Looks like Titus has moved from older method of installations that
involved a lot of configure and make and make install commands or make
followed by copying the contents of /bin directories to
/usr/local/bin(see
`here <http://ged.msu.edu/angus/2013-04-assembly-workshop/installing-software.html>`__
for example) to using a package manager on the Ubuntu systems. Since I
am trying to set up machines for Apri-May 2015 now, I am going try to
change things over to that. (I may leave some old notes I worked out.)
See the links above for guidance along the lines the older methods.

SAMtools
^^^^^^^^

`For
Ubuntu <http://angus.readthedocs.org/en/2014/amazon/technical-guide.html>`__

::

    apt-get install samtools

Bedtools
^^^^^^^^

`For
Ubuntu <http://bedtools.readthedocs.org/en/latest/content/installation.html>`__

::

    apt-get install bedtools

BWA
^^^

Looks like accoriding to
`here <http://nebc.nerc.ac.uk/bioinformatics/docs/bwa.html>`__ maybe
apt-get can install it.

::

    sudo apt-get install bwa

Worked.

Other information I found, besides the Mac installtion info, is
`here <https://answers.launchpad.net/ubuntu/+question/117555>`__ and
`here <http://superuser.com/questions/781350/trouble-installing-burrows-wheeler-aligner-linux>`__
and
`here <http://icb.med.cornell.edu/wiki/index.php/Elementolab/BWA_tutorial>`__

FastQC
^^^^^^

NOT DONE YET ON UBUNTU!!!

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

Bowtie2
^^^^^^^

FastX Toolkit
^^^^^^^^^^^^^

Items to note about the next steps:

-  libgtextutils NEEDS TO BE INSTALLED FIRST!! The FASTX-Toolkit relies
   on this and seems to look for related items during installation.

-  FastX Toolkit also needs pkg-config but it looks like that is
   installed already in ami-7606d01e, and so that should be all set

Note for UBUNTU system, preferable way is to let package manager handle
this and so looks like I can just use
`apt-get <http://manpages.ubuntu.com/manpages/lucid/man8/apt-get.8.html>`__.
See `here <https://www.biostars.org/p/84768/>`__

::

    sudo apt-get install fastx-toolkit

WORKED and seemed to install the dependencies at the same time
automatically.

In fact, the Hannon lab site has a link to `the installation
instructions for Ubuntu and
Debian <http://hannonlab.cshl.edu/fastx_toolkit/install_ubuntu.txt>`__
right on `the download and installation
page <http://hannonlab.cshl.edu/fastx_toolkit/download.html>`__ and the
first suggestion is to use APT to get the pre-requisites and then lists
commands to install libgtextutils first and then FastX Toolkit.

Alterntaively for other Unix systems, someone nicely posted a link the
full manual installation `for CentOS
here <http://hannonlab.cshl.edu/fastx_toolkit/install_centos.txt>`__ in
response to `someone posting about the same errors I was seeing when
trying to complete installation on my
Mac <http://seqanswers.com/forums/showthread.php?t=10709>`__ and this
was helpful as a guide to the Mac installtion as well.

SRA Toolkit
^^^^^^^^^^^

`SRA toolkit
downloading <http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software>`__

Ubuntu Linux version
''''''''''''''''''''

Best - get up to date version
                             

First go to '~' directory in your instance. ``/mnt`` is the scratch disk
space for Amazon machinesbut we are going to unpack the software in the
root directory so it remains there when instance stopped. This will
allow us to stop the instance to save money when not actively in use.

::

    cd ~

Follow
`here <http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=std>`__

While in home directory (cd ``~/``), start with step #2
``Download the Toolkit from the SRA website``. You can get the link to
use in the wget command by by using a computer that had a browser and
browsing to http://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/current . I saw in
the list that one began with ``u`` so I clicked on that to verify it was
ubuntu and copied the last part to combine with example in step 2 to
replace Centos version with Ubuntu version download.

::

    wget "http://ftp-trace.ncbi.nlm.nih.gov/sra/sdk/current/sratoolkit.current-ubuntu64.tar.gz"

Unzip download (step #1 under ``Unpack the Toolkit``)

::

    tar -xzf sratoolkit.current-ubuntu64.tar.gz

Deleted download to clean up. (Optional)

::

    rm sratoolkit.current-ubuntu64.tar.gz

Renamed directory to make building commands easier. (Optional but
subsequent commands have paths assuming you did it. Change to match your
directory hierarchy.)

::

    mv sratoolkit.2.4.5-2-ubuntu64/ sratoolkit

Ran command

::

    ./sratoolkit/bin/fastq-dump

Gave me usage information. Looked promising.

Tried test recommended at `SRA Toolkit Installation and Configuration
Guide <http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc&f=std>`__
page.

::

    ./sratoolkit/bin/fastq-dump -X 5 -Z SRR390728

They say:

    the test should connect to NCBI, download a small amount of data
    from SRR390728 and the reference sequence needed to extract the
    data, and stream the first 5 spots of the file ("-X 5" option) to
    the screen ("-Z" option).

If successful you should see a bit of data as they describe. It will
also create an ``ncbi`` directory within my directory and that had
``SRR390728.sra.cache`` under the directory ``~/ncbi/public/sra``.

apt-get
       

I STRONGLY ADVISE NOT USING THIS APPROACH!!! (directions only placed
here to document what was tried and in hope eventually it is this easy.)
I TRIED AND FOUND THIS DOWNLOADED AN OLD VERSION (fastq-dump was version
2.1.7 and there was no ``prefetch`` in ``/bin``) I COULDN'T SEEM TO GET
TO WORK. Can use apt-get according to
`here <http://installion.co.uk/ubuntu/saucy/universe/s/sra-toolkit/install/index.html>`__
and
`here <http://www.howtoinstall.co/en/ubuntu/utopic/universe/sra-toolkit/>`__,
but
`here <http://genomespot.blogspot.com/2015/01/sra-toolkit-tips-and-workarounds.html>`__
says not to do it this way as it will be old. I am going to try apt-get
route and see if works for what I need. (IT INDEED DID NOT WORK FOR ME
AS THE GENOMESPOT BLOG ADVISED.)

::

    apt-get install sra-toolkit

I STRONGLY ADVISE NOT USING THIS APPROACH!!! SEE ABOVE.

MACS2
^^^^^

When I search ``macs2`` I found it at https://pypi.python.org/pypi/MACS2
. The site being ``pypi.python.org`` indicated to me that I should be
able to use the package manager ``pip`` once installed on Ubunut to
easily download and install.

::

    pip install macs2

CEAS
^^^^

Acquiring from
`here <http://liulab.dfci.harvard.edu/CEAS/download.html>`__

::

    wget http://liulab.dfci.harvard.edu/CEAS/src/CEAS-Package-1.0.2.tar.gz

Unpacking and installing, following
`here <http://liulab.dfci.harvard.edu/CEAS/install.html>`__

::

    tar xvf CEAS-Package-1.0.2.tar.gz

    rm CEAS-Package-1.0.2.tar.gz

    cd CEAS-Package-1.0.2/

    python setup.py install

Sanity check.

::

    ceas

Listed usage and so it worked.

CEAS's ``build_genomeBG`` utility needs to access external databases so
I added ``python-mysqldb`` to the apt-get installation commands, similar
to advised
`here <http://ged.msu.edu/angus/tutorials-2011/chipseq-peakcalling-tutorial.html>`__.
(Actually, when I did that command after having instance already running
but having not run it before it said it was already installed. Maybe
something else I had already listed was dependent on it.)

MEME
^^^^

Not avialable via ``apt-get``.

Use on webserver `here <http://meme.nbcr.net/meme/tools/meme>`__.
(Supposedly `here <http://meme-suite.org/tools/meme>`__ is the most
up-to-date version of the site. However, the Upstate network said it was
unavailable or it violated policy and has been blocked when I submitted
jobs there.)

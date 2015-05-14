.. contents::
   :depth: 3.0
..

Initial steps condensed for today
=================================

In the interest of time we are going to vastly condense the first two
steps. I have done these steps on your instances already. Additionally,
we'll get the third one running ahead of before we'll really need the
results

So you now should have

::

    genome.fa genome.fa.fai SRR346368.fastq

in your ``/usr/workshop`` directory.

You can just enter ``ls`` to see this if you are already in the
``/usr/workshop`` directory.

If you are unsure what is your current working directory, type ``pwd``.
If in doubt about any of that, just enter the commands below in your
terminal

::

    cd usr/workshop
    ls

-  The ``genome.fa`` represents getting the genome data for the organism
   with which we'll be working.

-  The ``genome.fa.fai`` file has summarized information about the
   lengths of each chromosome that we'll use for today's work. The
   details of obtaining these files is in the early part of the
   pipeline.

-  The ``SRR346368.fastq`` is the experimental data. Because it is
   important to know what it is and where it came from for today's work,
   we'll look at that some.

Before that though, in the interest of time we are going to initiate a
rate-limiting step first and then build up to you understanding what you
did after.

Please enter in your terminal

::

    git clone https://github.com/fomightez/sequencework.git
    cp sequencework/SOLiD/split_SOLiD_reads.py .
    python split_SOLiD_reads.py SRR346368.fastq TAGCGT 10

Now we'll talk about the two big parts of steps #1 and #2 we skipped.
That should take us to the point of the commands we just ran and then
we'll actually continue with the workshop steps as documented, picking
up with the ``Mapping reads`` section.

Note that the instructions for steps #1 and #2 start off assuming you
have a clean, new system (the set up of which is described `here and the
pages after
that <http://fenglabwkshopmay2015.readthedocs.org/en/latest/technical_guide_to_software>`__
). This obviously isn't the case today and so we will not run those
commands as we talk about those steps.

.. contents::
   :depth: 3.0
..

Logging into your computer instance “in the cloud” (NON-windows version; Mac and Linux)
=======================================================================================

These instructions are written for a Mac operating system, but they
should work for Linux once you open your ``terminal`` program as well.

Connecting using the key and SSH
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You'll be provided with the network name, a.k.a. ``public DNS``, of your
virtual machine. This is the public name of your computer on the
internet.

Connect to that computer with ssh under the username ‘ubuntu’, as
follows.

If you prepared in advance, the key provided for today should be on your
Desktop already.

Next, start Terminal (on a Mac it is in ``Applications`` >
\`Utilities...) and type:

::

    chmod og-rwx ~/Desktop/workshop.pem

to set the permissions on the private key file to “closed to all
evildoers”. SSH will be protective and won't allow you to connect if you
skip this step. You only ever need to do this once for each key on your
computer.

Next type:

::

    ssh -i ~/Desktop/amazon.pem ubuntu@ec2-???-???-???-???.compute-1.amazonaws.com

(but you have to replace the stuff after the ‘@’ sign with the network
name of the computer that is provided to you. You may wish to paste the
template in a text editor and edit it first if it makes it easier for
you.)

With the above command, you’re using ssh to log in as user ‘ubuntu’ to
the machine ‘ec2-174-129-122-189.compute-1.amazonaws.com’, for an
example name, using the authentication key located in ``workshop.pem``,
a.k.a. the ``identity file`` prefaced by the ``-i`` option in the
command, on your Desktop.

You should now see a text line that starts with something like
``ubuntu@ip-10-235-34-223:~$``. You’re connected to the machine at
Amazon Web Service, but you have have one more important step.

Type:

::

    sudo bash
    cd /usr/workshop

The first command restarts the bash shell with you using as the super
user and the second sets you in the directory where we will work today
``usr/woshop``. You have to issue these commands each time you login; it
doesn't persist.

This is where we will begin today and it after this it will be much like
you are working on your own computer's command line.

To check out what we have you can type the command below to see

::

        df -h

Exiting (probably won't need to do this today)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

To log out, type:

::

    exit
    logout

or just close the terminal or Putty window. (You cannot do this step
wrong because ultimately you (or me, for today) have control of the
instance in Amazon Web Services console.)

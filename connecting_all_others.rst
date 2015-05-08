.. contents::
   :depth: 3.0
..

Logging into your computer instance “in the cloud” (NON-Windows version; for Mac and Linux)
===========================================================================================

These instructions are written for a Mac operating system, but they
should work for Linux once you open your ``terminal`` program as well.

I will step through this process using a Mac at the beginning of the
workshop.

Connecting using the key and SSH
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You'll be provided with the network name, a.k.a. ``public DNS``, of your
virtual machine. This is the public name of your computer on the
internet. It will look something like
``ec2-174-129-122-189.compute-1.amazonaws.com``, for example.

Connect to your particular virtual computer with ssh under the username
``ubuntu``, as follows.

If you prepared in advance, the key provided for today should be on your
Desktop already.

Next, start Terminal (,on a Mac it is in ``Applications`` >
``Utilities``,) and type:

::

    chmod og-rwx ~/Desktop/workshop.pem

to set the permissions on the private key file to “closed to all
evildoers”. SSH will be protective and won't allow you to connect if you
skip this step. You only ever need to do this once for each key on your
computer.

Next type:

::

    ssh -i ~/Desktop/workshop.pem ubuntu@ec2-???-???-???-???.compute-1.amazonaws.com

(but you have to replace the stuff after the ‘@’ sign with the network
name of the computer that is provided to you. You may wish to open your
text editor and paste the template in the text editor for editing if it
makes it easier for you because you can point and click there.)

With the above command, you’re using ssh to log in as user ``ubuntu`` to
the machine ``ec2-174-129-122-189.compute-1.amazonaws.com``, for an
example name, using the authentication key located in ``workshop.pem``,
a.k.a. the ``identity file`` prefaced by the ``-i`` option in the
command, on your Desktop.

The first time you try this on your own computer it will say it is the
first time seeing the machine you are trying to connect to and ask if
you'd like to register to use this computer. Simply type ``yes`` at the
prompt.

::

    $ yes

Some information should scroll by and at the bottom you should now see a
text line that starts with something like
``ubuntu@ip-10-235-34-223:~$``. You’re now officially connected to the
machine at Amazon Web Services. Hooray! But you have have one last,
IMPORTANT step to totally take control.

Type:

::

    sudo bash
    cd /usr/workshop

The first command restarts the bash shell with you using the machine as
the super user and the second sets you in the directory where we will
work today ``usr/workshop``. You have to issue these commands each time
you login; these settings don't persist.

This is where we will begin today and it after this it will be much like
you are working on your own computer's command line.

OPTIONAL: To check out what you have you can type the command below to
see

::

        df -h

Exiting
~~~~~~~

(You probably won't need to do this today.)

To log out, type:

::

    exit
    logout

or just close the terminal or Putty window. (You cannot do this step
wrong because ultimately you (or me, for today) have control of the
instance in Amazon Web Services console.)

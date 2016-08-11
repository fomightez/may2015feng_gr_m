.. contents::
   :depth: 3.0
..

Appendix: Help getting files on and off Amazon AWS EC2 instances
================================================================

SCP
---

Limited to linux/Unix/Mac systems, but very straighforward.

-  http://angus.readthedocs.org/en/2014/amazon/transfer-files-between-instance.html

Example that worked in the terminal on my Mac to download from my Amazon
instance (back when public DNS/hostname formatted like
ec2-50-19-16-34.compute-1.amazonaws.com).

::

    scp -i workshop.pem ubuntu@ec2-50-19-16-34.compute-1.amazonaws.com:/usr/workshop/sc3.db .

The key is ``workshop.pem``. And
``ec2-50-19-16-34.compute-1.amazonaws.com`` was my current instance
hostname. The ``.`` specified to download the file ``sc3.db`` to the
current working directory.

For newer Amazon EC2 instances (Summer 2015 forward-->), that command
will look more like

::

    scp -i workshop.pem ubuntu@54.86.18.122:/usr/workshop/sc3.db .

Where ``54.86.18.122`` is the public IP address of the instance.

WARNING: You may observe errors like
``scp: /root/my_directory/genome.fa: Permission denied`` if you try to
download/upload to places without proper permissions. Check the
permissions (command ``ls -l``) for learning which directories you can
read and write to on your instance for using scp to upload/download, see
`here <http://ss64.com/bash/syntax-permissions.html>`__ for interpreting
the codes. (Basically you want ``r`` for downloading and ``w`` for
uploading to be among the six characters on the right). For example, you
cannot read or write direct to ``root``. Aside from ``root``, most
directories created by the instance at creation, even those below
``root`` allow reading which makes downloading from most directories to
a local drive possible using ``scp``. Tip: for Amazon EC2 instances
there is a ``tmp`` directory that allows both reading and writing. For
uploading to your instance you can use that ``tmp`` as a target to go
from your local machine to your instance. Then once uploaded, move the
file or files to a more logical place within your instance.

See also:

-  http://stackoverflow.com/questions/6558080/scp-secure-copy-to-ec2-instance-without-password

-  http://stackoverflow.com/questions/10364950/uploading-files-on-amazon-ec2

SFTP
----

Most likely best choice if you are on Windows.

Pay attention to the user name. You want ``ubuntu`` for Ubuntu
instances. Those links below should also provide information if you
happen to be using other types of AWS EC2 instances. Be sure to read the
comments too. They often give you nice pointers when you are
troubleshooting.

with FileZilla
^^^^^^^^^^^^^^

-  `Example on Windows
   PC <http://angus.readthedocs.org/en/2014/amazon/transfer-files-between-instance.html>`__

-  `Example on Mac <https://www.youtube.com/watch?v=e9BDvg42-JI>`__ with
   more information at `Yasitha Chinthaka's post
   here <http://stackoverflow.com/questions/16744863/connect-to-amazon-ec2-file-directory-using-filezilla-and-sftp>`__

-  `Another take on this is
   here <https://mdahlman.wordpress.com/2012/03/21/filezilla-and-ec2-using-private-keys/>`__

with cyberduck
^^^^^^^^^^^^^^

-  `Example on Mac <https://www.youtube.com/watch?v=hd4oL3WIPVM>`__

Dropbox
-------

Looks like it still works since in 2014 course info

http://angus.readthedocs.org/en/2014/amazon/installing-dropbox.html

BUT BE WARY!!!

    IMPORTANT: Dropbox will sync everything you have to your EC2
    machine, so if you are already using Dropbox for a lot of stuff, you
    might want to create a separate Dropbox account just for the course.

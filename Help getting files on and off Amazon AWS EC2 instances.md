# Help getting files on and off Amazon AWS EC2 instances

## SCP

Limited to linux/Unix/Mac systems, but very straighforward.

* http://angus.readthedocs.org/en/2014/amazon/transfer-files-between-instance.html

Example that worked in the terminal on my Mac to download from my Amazon instance.

	scp -i workshop.pem ubuntu@ec2-50-19-16-34.compute-1.amazonaws.com:/usr/workshop/sc3.db .

The key is `workshop.pem`. And `ec2-50-19-16-34.compute-1.amazonaws.com` was my current instance. The `.` specified to download the file `sc3.db` to the current working directory.



* http://stackoverflow.com/questions/6558080/scp-secure-copy-to-ec2-instance-without-password

* http://stackoverflow.com/questions/10364950/uploading-files-on-amazon-ec2


## SFTP

Most likely best choice if you are on Windows.

Pay attention to the user name. You want `ubuntu` for Ubuntu instances. Those links below should also provide information if you happen to be using other types of AWS EC2 instances. Be sure to read the comments too. They often give you nice pointers when you are troubleshooting.

#### with FileZilla

* [Example on Windows PC](http://angus.readthedocs.org/en/2014/amazon/transfer-files-between-instance.html)

* [Example on Mac](https://www.youtube.com/watch?v=e9BDvg42-JI) with more information at [Yasitha Chinthaka's post here](http://stackoverflow.com/questions/16744863/connect-to-amazon-ec2-file-directory-using-filezilla-and-sftp)

* [Another take on this is here](https://mdahlman.wordpress.com/2012/03/21/filezilla-and-ec2-using-private-keys/)

#### with cyberduck

* [Example on Mac](https://www.youtube.com/watch?v=hd4oL3WIPVM)



## Dropbox

Looks like it still works since in 2014 course info

http://angus.readthedocs.org/en/2014/amazon/installing-dropbox.html

BUT BE WARY!!!

>IMPORTANT: Dropbox will sync everything you have to your EC2 machine

<img src="/blog/uploads/enfs-combo.jpg" style="float:right;"/>

Security is a thing a my [new job][idw], unlike many other places i
have worked.  We follow PCI security standards.  We take great care
never to have our customers sensitive information on unsecured
machines.  We try to stay aware of the security risks in our
environments.  With that in mind, i decide that storing all my work
related files on my laptops in clear text was suboptimal.  I have
thought at pretty much every job I have had, but i never did anything
about it before.

After some research i settled on [EncFS][].  Unlike the other
encrypted filesystems EncFS does not require you reserve a large
amount of disk space up front.  EncFS effectively lets you make a
directory in which all the files will be encrypted.  This is great for
documents, source code, temp files, etc.

In addition to normal sorts of files, i utilize databases quite a bit.
All my test and development databases need to be encrypted on disk.
These databases don't contain any customer data they do contain some
information that my employer would prefer not be public knowledge.

The encrypted directory is not readable until i log in and provide a
password.  That means that when the database server starts it cannot
access the database files on disk.  Fortunately, [PostgreSQL][pg] is
totally bad ass.  It will happily start up even if it is unable to
access some of its configured table spaces.  As soon as the encrypted
filesystem is mounted, the databases that reside in the encrypted
directory instantly become available.  The encryption layer does not
even effect performance noticeably.[^dev-only]

[^dev-only]: This is light duty single user performance we are talking
  about.  I wouldn't suggest this setup for a heavy load production
  environment, but in development it is great.

That combined with a tiny script that unmounts the encrypted
filesystem when my machine is suspended, means that if someone steals
my laptop the only thing they can access is my family photos.  That is
a pretty nice feeling.  It turned out to be so easy i wish i had not
waited until now to figure out how to do it.




[pg]: http://www.postgresql.org/
[fuse]: http://fuse.sourceforge.net/
[ubuntu]: http://www.ubuntu.com/
[encfs]: http://www.arg0.net/encfs
[idw]: http://www.idwatchdog.com/
[pci]: https://www.pcisecuritystandards.org/
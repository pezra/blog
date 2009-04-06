<img src="/blog/uploads/encfs-combo.jpg" style="float:right;"/>

Security is a thing at my [new job][idw].  We follow [PCI security
standards][pci].  We take great care never to have our customers
sensitive information on unsecured machines.  We make efforts to stay
aware of the security risks in our environments.  

With that in mind, i decide that storing all my work related files on
my laptops in clear text was suboptimal.  I have thought this same
thing at pretty much every job I have had, but i have never done
anything about it before.

After a bit of research i settled on [EncFS][] as the best mechanism
to encrypt my work related data.  It is an encrypted file system.
Unlike the most of the other encrypted file systems for Linux, EncFS
does not require reserving large amounts of disk space up front.
EncFS effectively lets you make a directory on an existing file system
in which all the files will be encrypted.  It is very easy to setup
and use.

In addition to normal sorts of files, i utilize databases quite a bit.
All my test and development databases need to be encrypted on disk.
These databases don't contain any customer data they do contain some
information that my employer would prefer not be public knowledge.

The encrypted directory is not readable until i log in and provide a
password.  That means that when the database server starts it cannot
access the database files on disk.  Fortunately, [PostgreSQL][pg] is
totally bad ass.  It will happily start up even if it is unable to
access some of the configured table spaces.[^public-mode]  As soon as
the encrypted file system is mounted, the databases that reside in the
encrypted directory instantly become available.  The encryption layer
does not even effect performance noticeably.[^dev-only]

[^dev-only]: This is light duty, single user, performance we are
  talking about.  I wouldn't suggest this setup for a heavy load
  production environment, but in development it no sweat.

[^public-mode]: To allow the `postgres` user to access the encrypted
  file system you do need to mount it with the `--public` option.

One thing i did think is a little weak is that encrypted file systems
don't get unmounted when the computer is put to sleep.  No worries,
though.  A tiny script in `/etc/pm/sleep.d` to unmount the file system
is all it takes to rectify that situation.

Now if someone steals my laptop the only thing they will be able to
access are my family photos.  That is a pretty nice feeling.  Even
better, it turned out to be very easy.




[pg]: http://www.postgresql.org/
[fuse]: http://fuse.sourceforge.net/
[ubuntu]: http://www.ubuntu.com/
[encfs]: http://www.arg0.net/encfs
[idw]: http://www.idwatchdog.com/
[pci]: https://www.pcisecuritystandards.org/
The idea of creating a functioning application by loosely connecting
many small pieces has been around for a long time.  Certainly since
early in the development of Unix, and probably even before.  It has
survived because it is such a powerful approach.  

This idea is at the very core of the architecture of the [web][splj].
However, achieving such compartmentalization has been difficult for
business application development.  The recent advancement of REST into
the mainstream is bringing this mentality within the reach of many
development teams.

The small pieces approach totally dominates the lower levels of
software development in the form of object oriented programing.  Each
"object" is a small self-contained piece, and a large number of the
small pieces are joined together to provide the functionality of the
application.

At the application level, however, this approach does not enjoy the
same ubiquity.  It is much more common to see a monolithic approach.
There is one giant application that does everything for everyone.  The
path of least resistance for any single new feature is to implement it
in the existing application structure.  There is some overhead in
creating a new application, so in the short term multiple small
applications seems more costly.

Unfortunately, while it is easier to add any particular feature to an
existing application doing so means you give up all the advantages of
small pieces loosely joined.  And, the advantages of a small application
approach are significant.

Perhaps the single largest advantage is that smaller applications are
easier to understand.  To effectively improve or maintain software it
is necessary to understand it.  The [connascence][] of one bit of code
and everything else is often unclear.  This lack of clarity means that
the risk of any particular change having unintentional impacts
increases with the size of the code base.  Building more smaller
applications is an effective way to manage such risks.

Another advantage of this approach is that it makes using novel
implementation techniques more workable.  If you have a large
monolithic application and you get a new requirement that might
benefit from a different language, framework or runtime you are pretty
much out of luck.  In a compartmentalized architecture each
application can have it's own technology stack.  If you new a set of
features that that might benefit from the concurrency of Erlang you
can use for that component without impacting the other components in
any way.

Have several small applications often turns intractably large efforts
in to several smaller tasks.  For example, consider upgrading the
framework on which your application(s) are built to a new version.
The new version has features and improvements that would be highly
beneficial but target version is incompatible with the version you are
currently running in some minor ways.

Such an upgrade will necessarily touch most of the application.  Its
risk profile will be very broad.  The benefits of the upgrade will
rarely be directly visible to the business so the priority of such
work is always rather low.  The cost and risk of such work is often so
large, and the perceived benefit so small, that such work is put off
until support is being terminated for the version currently in use.

In a compartmentalized system the any single component can be upgraded
much more quickly, and at lower risk.  The benefits of upgrading match
the effort required much more closely in such situation.  Many times
the effort required is often so low there is no discussion required,
upgrades become normal refactoring tasks.

Experimentation is also quite a bit more manageable in a
compartmentalized architecture.  The smaller size of the components
makes implementing new ideas faster and more approachable.  This
lowers the cost of implementing new ideas and recovering if the idea
does not pan out.

When experimenting it is often not immediately clear if a new idea
really is an improvement.  Sometimes developers and users need to work
with it a for a while to form a reasoned opinion.  In
compartmentalized systems experiments can be designed to impact a
small portion of the total application.  This allows small experiments
to can soak for a while until the team is ready to call the results.
If idea worked the practice can be expanded to the rest of the
components, if not it is only a small portion of the code base that
needs to be cleaned up.

It is worth noting that this approach will effectively true your large
application into a distributed system of small applications.  This is
distributed applications are a little scary, and for good reason.
Before you embark on this path you should have a plan for how to
integrate the parts into a whole.  For most business application
[REST][]/HTTP is a very good technology for integrating applications.

There are many other situations where the small pieces approach's
conversion of large tasks into small ones is an advantage.  There are
also situations where it causes more overall work.  In my experience,
though, the chunking of tasks is well worth the small additional
overhead.  It is much easier to manage many small semi-independent
development efforts than a few large ones.  




 






[rest]: http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm
[connascence]: http://onestepback.org/articles/connascence
[splj]: http://smallpieces.com

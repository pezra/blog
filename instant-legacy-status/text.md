<img src="http://barelyenough.org/blog/uploads/msgp-api.jpg" alt="MS Great Plain API diagram" style="float:right;"></img>

That is what you gain the moment you allow more than one module of
software to access any single database.  Any
[integration database][int-db] confers legacy status on all modules that access it.

For the sake of this discussion we will define a module as unit of
software that is highly cohesive, logically independent and
implemented by a single team.  A module is usually a single code base.
Practically speaking, module boundaries usually depend on 
[Conway's Law][conway-law].  However, disciplined teams can gain some advantage
from sub-dividing their problem space into multiple modules.

[conway-law]: http://en.wikipedia.org/wiki/Conway%27s_Law

Pardon me while i digress...
-----------------------------------

I recently attended [Mountain West RubyConf][mwrc].  While there i had
the pleasure of hearing a brilliant talk by [Jim Weirch][jim-weirch]
about [connascence in software][connascence]. ([Here][conn-alt1]
[are][conn-alt2] some other write ups of the same talk, but from
different conferences.)  In brief, a unit of software, A, is
connascence with another, B, if there exists some change to B that
would necessitate a change in A to preserve the overall correctness of
the system.

[jim-weirch]: http://onestepback.org/
[conn-alt1]: http://urgetopunt.com/2009/03/27/sor-connascence.html
[conn-alt2]: http://deadprogrammersociety.blogspot.com/2009/04/larubyconf-2009-jim-weirich-grand.html
Connascence has various flavors.  Some of these variations cause more
maintenance problems than other.  Connascence of Name is the practice of
linking code by name.  For example, calling some method `foo`.
Obviously a form of connascence but one which we regularly accept.
Connascence of Algorithm is the linking of code by requiring both
pieces use the same algorithm.  When we see this in practice we
generally run from the room screaming "[DRY][]".

[dry]: http://en.wikipedia.org/wiki/Don%27t_repeat_yourself

Many on the thing we like in software reduce connascence.  Likewise,
many of the things we regard as [code smells][code-smell] result in
increased connascence.  Lower levels, and degree, of connascence are
desirable because it reduces the effort required to change software.
Mr Weirch posited that connascence might be a basis for a unified
theory of software quality.  It is definitely the most comprehensive
framework i am aware of for thinking, and communicating, about the
quality of so software.


Back on point
------------

It is clear that a database makes all the modules that access it
connascent with one another.  Many changes to an application will
require changes to the database; any changes to the database will
require changes to the other modules to maintain overall correctness.
All changes to the database require that the correctness of all
accessing modules be verified.  Such verification is often
non-trivial.

The integration database is particularly problematic because it forces
a high degree of a variety of forms of connascence.  It causes
Connascence of Name.  All modules involved need to agree on the names
of the columns, tables, database, schema and host.  Right off the bat
you should start to wonder about this approach. The modules are weakly
connascent at many points.

If the data model is normalized it may well avoid introducing
Connascence of Meaning.  If, on the other hand, you have implemented
any enumerable values without providing look-up tables; have logically
boolean fields stored as numeric types; or have used a wide variety of
common practices you will have introduced some Connascence of Meaning.
Connascence of Meaning requires that the units agree on how to infer
meaning from a datum (e.g., in this context 1 means true).  It is a
stronger form of connascence than Connascence of Name.

While we are on this point, remember that if your mad data modeling
skills did manage to avoid significant Connascence of Meaning you did
it by adding significant amounts of Connascence of Name.  That is what
a lookup-table is.

So far not so good, but that that was the good part.  The really
disastrous part is that by integrating at the database level you are
required to introduce staggering levels of Connascence of Algorithm.
Connascence of Algorithm is very strong form a connascence.  Allowing
one module to interact with another's data storage means that both
modules have to understand (read: share the same algorithm) the
business rules of the accessed module.  If the business rules change
in a module, the possibility exists that any other module that
accesses the data might now operate incorrectly.


Only application databases need apply][app-db]
------------------------------------------

I fall squarely on the side of not using databases as integration
vectors.  The forms and degree of connascence that such an approach
introduces make it recipe for disaster.  Such high level of
connascence will raise the costs of change (read: improvment) in all
the modules and database involved.  Application systems tend to get
more integrated over time so the cost of improvement rises rapidly in
systems that use integration databases.

After a while the cost of change will become so high that only
improvements that provide huge value to the business will be worth the
cost.  For weak, undisciplined teams this will happen very rapidly.
For strong, smart and highly disciplined teams it will take a bit
longer, but it will happen.  Once you allow more than one module to
access your app's database forever will it dominate your destiny.


[app-db]: http://martinfowler.com/bliki/ApplicationDatabase.html
[int-db]: http://martinfowler.com/bliki/IntegrationDatabase.html
[connascence]: http://docs.rubyrake.org/articles/connascence/softwareconnascence.html
[code-smell]: http://c2.com/xp/CodeSmell.html
[mwrc]: http://mtnwestrubyconf.org/2009/
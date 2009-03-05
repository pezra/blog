A *nucleic team* is one with small core group of permanent employees,
usually just 1 to 3 people, that is supplemented as needed by
contractors.  The core in a nucleic team is too small to do the
anticipated work, even during slow periods of development.  The core
teams job two fold, first it is to implement stories that are
particularly complicated, risky or architecturally important.  The
second part of the core teams job is to manage a group of contractors
by creating statements of work, doing code reviews, etc.

The nucleic structure should provide a lot of advantages from a
business stand point.  You get many of the benefits of having an
in-house development team.  Advantages like developers that have the
time and incentives to become domain experts.  A consistent group of
people with which all the stakeholders can build a rapport.  A group
of people that work together long enough to build the shared vision it
takes to create systems with conceptual integrity.[^mmm]

Those advantages are combined with the advantages of pure contracting
team, at least in principle.  The primary advantages of a pure
contracting are that you can scale the development organization, both
up and down, rapidly and cost effectively.  Many organizations with
in-house development teams end up having to maintain a sub-optimally
sized development team.  Work loads and cash flow tend to vary a bit
over time.  It takes a long time to find and hire skilled developers.
Once you do, it really sucks to have to lay people off, either because
of the lack of work or lack of money.  Resizing development teams is
so costly and disruptive that most organizations tend to pick a team
size that is larger than optimal for the slow/lean times but less than
optimal for the plentiful times.

Risks
------

This structure is not without it risks, though.  Finding talent
contractors is not easy.  Contractors, by their very nature, cannot be
relied on when planning beyond their current contract.  Most
importantly, though, contracting usually has an incentive structure
that favors short term productivity.  All of these can threaten the
long term success of project if not managed correctly.

To counteract the risks inherent in contract workers the core team
must be committed to the business, highly talented and fully empowered
by the executive team to aggressively manage the contractors.  The
core team members must be highly skilled software developers, of
course, but this role requires expertise in areas that are
significantly different from traditional software development.  The
ability to read and understand other peoples code rapidly it of huge
importance.  As is the ability to communicate with both the business
and the contractors what functionality is needed.  The core team also
needs to be able to communicate much more subtle, squishy, things like
the architectural vision and development standards.

The core team will not be as productive at cutting code as they might
be use to.  The core team role is not primarily one of coding.  A
significant risk is that the members of the core team might find that
they do not like the facilitation and maintainership role nearly as
much as cutting code.  It is necessary to set the expectations of
candidates for the core team appropriately.  One other risk is that
the core team will get so bogged down in facilitation and
maintainership tasks that they actually stop cutting code.  The
"non-coding architect" is a recipe for disaster, and should be avoided
at all costs.

While this team structure has much going for it, it will be
challenging to make work in practice.

Origins
-------

I think this team structure is developing in the Rails community out
of necessity, rather than preference.  Rails is a highly productive
environment.  That can make it a competitive advantage for
organizations that use it.  However, the talent pool for Ruby and
Rails is rather small.  Additionally, many of the people who are
highly skilled at Rails prefer to work as contractors.  The percentage
of the Rails talent pool that prefers to be independent seems quite
high by comparison to any other community i know of.

This raises a problem for organizations that would like to create an
in-house development team using Rails.  Most of the talent would
rather not work for you, or anyone for that matter.  However, if you
can build a small core team to manage the development and hold the
institutional knowledge for the project you can utilize the huge
talent that exists in the Rails contractor community to drive the
project to completion.

I am not sure if this structure and the reason behind it are good, or
bad, for the Rails community as a whole.  The nucleic team model might
turn out to be a competitive advantage in itself because it embodies
the benefits of both internal and external development teams.  On the
other hand, it is bound to be a bit off putting for organizations that
are not use to it.


[^mmm]: See Mythical Man Month by [Fred Brooks](http://www.cs.unc.edu/~brooks/) for more details on the importance of conceptual integrity.
I am a fan of [polylithic architectures][small-pieces].  Such
architectures have many advantages related to enhancing evolvability
and maintainability.  When you decide to create a system composed of
small pieces how do you decide what functionality goes into which
component?

[small-pieces]: http://barelyenough.org/blog/2009/09/small-pieces/

Principles 
------

The goal is to sub-divide the application into multiple
[highly cohesive][cohesion] components which are weakly
[connascence][] with each other.  To achieve the desired cohesion it
will be necessary to align the component boundaries with natural
fissure points in the application.

[cohesion]: http://en.wikipedia.org/wiki/Cohesion_(computer_science)
[connascence]: http://github.com/jimweirich/presentation_connascence/raw/master/Connascence.key.pdf

The strategy should allow for the production of a arbitrary number of
components.  A component that was of a manageable size yesterday could
easily become too large tomorrow.  In that situation the over-sized
component will need to be sub-divided.  Applying the same strategy
repeated will result in a system that is more easily understood.

We want to minimize redundancy in the components.  Redundancy results
in more code with must be understood and maintained.  More importantly
redundancy usually introduces [connascence of algorithm][coa], making
changes more error prone and expensive.  In a perfect world, any
particular behavior would be implemented in exactly one component.

We want to isolate changes to the system.  When implementing a new
feature it is desirable to change as few components as possible.  Each
additional component that must be changed raise the complexity of the
change.  The componentization strategy should minimize the number of
components involved in the average change to the system.

[coa]: http://onestepback.org/articles/connascence/conalgorithm.html

With those metrics in mind lets explore the two most common approaches
and see how they compare with each other.  Those two patterns of
componentization are horizontal slicing and vertical slicing.

Horizontal slicing
-----

In this approach the component boundaries are derived from that
implementation domain.  The implementation is divided into a set of
stacked layers in such a way that a layer initiates communication with
the layers below it.  This results in a standard layered
architectures.  By implementing each layer in a separate component you
can achieve the horizontal slicing.  This style of componentization
strategy results in the very common
[n-tier architecture pattern][tiered-arch].

[tiered-arch]: http://en.wikipedia.org/wiki/Multitier_architecture

For example, an application that has a business logic and a
presentation layer the application would be divided into two
components.  A business logic component and a presentation component.

Vertical slicing
----

In this approach the component boundaries are derived from the
application domain.  Related domain concepts are grouped together into
components.  Individual components communicate with any other
components as needed.  

This approach is also quite common but is usually thought of a lot
less formally.  It is more common for this type of segmentation to
develop incidentally.  For example, because separate teams developed
the parts independently, and then integrated them later.  Any time you
integrate separate applications you have vertical componentization.

The Score
---------

Against the metrics we laid out earlier, vertical slicing does much
better than horizontal.

|                  | Horizontal slicing | Vertical slicing |
|------------------|--------------------|------------------|
| Cohesion         | high               | high             |
| Repeatability    | low                | high             |
| DRYness          | low                | high             |
| Change isolation | low                | high             |


### Cohesion

Horizontal slicing has high cohesion.  Each of the components can
represent the a logically cohesive part of the implementation.

Vertical slicing also has high cohesion.  Each component represents
highly cohesive part of the application domain.
        
### Repeatability 

Vertical slicing provides a mechanism for reapply the subdivision
pattern an arbitrary number of times.  If any component gets too large
to manage it can be divided into multiple components based on the
application domain concepts.  This same process can be repeated from
the initial division of a monolithic application until components of
the desired size have been achieved.

Horizontal slicing is less repeatable.  The more tiers the harder it is
to maintain cohesiveness.  In practice it is very rare to see an
tiered architecture with more than 4 tiers, and 3 tiers is much more
common.

[dry]: http://en.wikipedia.org/wiki/Don%27t_repeat_yourself

### [DRYness][dry]

Horizontal slicing tends to result in some repetition. Certain
behaviors will have to be repeated a each layer.  For example, data
validation rules.  You will need those in the presentation layer to
provide good error messages and in the business logic layer to prevent
bad data being persisted.

Vertical slicing allows you to reduce the connascence of algorithm
because any single user activity is implemented in exactly one
component. Components usually do end up communicating to each other,
however, they do so in a way that does not require in the same
algorithms be implemented in multiple components.  For any one bit of
data or behavior, one component will its authoritative source.

### Change isolation

Vertical scaling tends to allow new features to be implemented by
changing only one component.  The component changed is the one which
already contains features cohesive with the new one.

Horizontal slicing, on the other hand, tends to require changes in
every layer.  The new feature will require additions to the
presentation layer, the business logic layer and the persistence
layer.  Having to work in every layer increase the cognitive load
required to achieve the desired result.

Conclusion
-----

Vertical slicing provides significant advantages.  The high cohesion,
dryness, and change isolation combine to drastically reduces the risks
and cost of change.  That is turn allow better/faster maintenance and
evolution of the system.  The repeatability allows you to retain these
benefits even while adding functionality over time.  Each time a
component gets too large you can divide it until you have reach a
application size that is human scaled.

Having a large number of components operate as a system does result in
a good deal of communication between the components.  It important to
pay attention to the design of the APIs.  Poor API design can
introduce excessive coupling which will eat up most of the advantages
described above.  [Hypermedia][] -- or more precisely, following the REST
architectural style -- is the best way i know to reduce coupling
between the components.

[hypermedia]: http://barelyenough.org/blog/2007/05/hypermedia-as-the-engine-of-application-state/
[REST]: http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm



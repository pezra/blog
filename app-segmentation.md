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

We want to minimize redundancy in the components.  In a perfect world,
an particular behavior would be implemented in exactly one component.
Any redundancy, obviously, results in more code with must be
understood and maintained.  More importantly redundancy often
introduces [connascence of algorithm][coa], making changes more error
prone and expensive.

[coa]: http://


There are two basic forms of componentization horizontal slicing and
vertical slicing.  With these metrics in mind lets explore these
approaches and see how they compare with each other.


### Horizontal slicing

In this approach the component boundaries are derived from that
implementation domain.  The implementation is divided into a set of
stacked layers in such a way that a layer initiates communication with
the layers below it.  This results in a standard layered
architectures.  By implementing each layer in a separate component you
can achieve the horizontal slicing.  This style of componentization
strategy results in the very common
[n-tier architecture pattern][tiered-arch].

[tiered-arch]: http://en.wikipedia.org/wiki/Multitier_architecture

For example, an application that has a business logic and
a presentation layer the application would be divided into 2
components.  A business logic component and a presentation component.  

### Vertical slicing


In this approach the component boundaries are derived from the
application domain.  Related domain concepts are grouped together into
components.  Individual components communicate with any other
components as needed.  Any time you have multiple applications that
communicate with each other you have vertical componentization.

This approach is also quite common but is usually thought of a lot
less formally.  For example, you might have multiple apps which
interact because multiple teams developed them independently.  Or
because the apps were developed at different times and where
integrated with each other later.


The Score
---------

|               | Horizontal slicing | Vertical slicing |
|---------------|--------------------|------------------|
| Cohesion      | high               | high             |
| Repeatability | low                | high             |
| DRYness       | low                | high             |



### Cohesion

Horizontal slicing has high cohesion.  Each of the components can
represent the a logically cohesive part of the implementation.

Vertical slicing also has high cohesion.  Each component represents
highly cohesive part of the application domain.
        
### Repeatability 

Vertical slicing provides a mechanism for reapply the subdivision
principle an arbitrary number of times.  If any component gets too
large to manage it can be divided into multiple components based on
the application domain concepts.  This same process can be repeated
from the initial division of a monolithic application until components
of the desired size have been achieved.

Horizontal slicing is less repeatable.  The more tiers the harder it is
to maintain cohesiveness.  In practice it is very rare to see an
tiered architecture with more than 4 tiers, and 3 tiers is much more
common.

### DRYness

Horizontal slicing tends to result in some repetition. Certain
behaviors will have to be repeated a each layer.  For example, data
validation rules.  You will need those in the presentation layer to
provide good error messages and in the business logic layer to prevent
bad data being persisted.

Vertical slicing 




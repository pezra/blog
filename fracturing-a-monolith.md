### outline

* data locality
  * group behavior based on data locality
  * build rough component vision early

* Auth first
  * central to everything
  * comprehensible
  * permission, no role, based

* existing seams
  * after auth find a seam in existing code to exploit
  * can be partial extraction

* new features
  * new features in new components
  * don't shared DBs!





Micro-service architectures are all the rage these days. Let's say, totally hypothetically, that you show up at a new job and find a large code base that has all the pathologies we have come to expect from monoliths. Your first thought might be something like "we should break this behemoth up into a collection of components, each of which is comprehensible to mere humans." That is a great goal, but how do you get from a huge, highly coupled, monolith to a constellation of cooperating components? Most "efforts" to modularize monolithic software are abandoned at about the time someone serious considers that question. That is before the effort even starts. It is easy to become overwhelmed by the immensity of the task and absence of obvious starting points and to just give up without a fight.

I have lead several successful decomposition efforts over my career. Each of these efforts shared some common patterns which, i believe, are the keys to success. These patterns are:

* commitment
* componentize based on data locality
* extract auth first
* facture along existing seams
* new features in new components

## Commitment

Decomposing a significant monolith *will* take time. It took tens or hundreds of man years to build the monolith. Breaking it up is going to take a while. Accept it. Plan for it. Remind yourself, and others, of this fact regularly. Patience is the only way to succeed. Some days, or weeks, or months, it will seem like you are never going reach your goal. In the moment, it will look like you are barely making dent, but if you have patience and persistence you will, over time, extract significant functionality into easier to maintain components.

> Most people overestimate what they can do in one year and underestimate what they can do in ten years.
> -- Bill Gates

You will never get done, though. Like any interesting project there is always more to do. Strive for progress, not perfection. The goal is not completion. Rather it is winning this, and the next, round of the repeating game. Winning this round implies shipping new features. Winning the next round implies leaving the architecture better than you found it.

All of the above applies to the entire development team, including management, not just the individual who initiates the decomposition. If management isn't on board it will be difficult, or impossible, to sustain the effort need to make real progress. If the engineering team as a whole is not committed, progress will stop with the loss of a key team member. The goal is to build a organization that will continuously chip away at the monolith.

## Componentize based on data

Perhaps the most ubiquitous concern for any service architecture is that of performance. Poor performance results in higher compute costs, reduced through put, and increased latencies. Performance is always an issue with services because the architectural style is based on inter-process communication (IPC). IPC requires IO and IO is [*way* slower than memory access](latency). The harsh reality is that it is harder to keep a service architecture performant.

[latency]: https://gist.github.com/jboner/2841832

One approach that can mitigate performance concerns is to co-locate the data and code involved in important operations in the same component. In practice, this means designing the component boundaries such that the component that implements a particularly important service is also the system of record for the data used by the service. Obviously, data locality must be balanced with componentization if we are to break up a monolith. It is important to consider the importance of any particular operation. Services that are depended upon by many other services, or that are used in time critical parts of the application should be co-located with the data they use. Services that are used less frequently, or in less critical places can use data from other services.

This is just a rule of thumb. Any real system will have situations that simply don't allow for the preferred data locality for all operations. Engineering is all about trade offs. In these situations caching becomes critical. HTTP, my preferred service protocol, provides sophisticated support for caching. Send time thinking about cache lifespans and invalidation when designing the resources of an API. Doing so will allow clients to more effectively and safely cache the data exposed. That will, in turn, improve the perceived performance and reduce the compute needs of the system. No operation is faster that than the one you avoid altogether. A well designed caching strategy allows avoiding many operations.

## Extract auth first

The very first thing to extract into a separate component, is authorization and authentication. Authorization is needed by basically everything (though it doesn't always require a separated component[^capability-based-authorization]), including all the services that will be extracted from the monolith. Authorization and authentication have good, widely supported standards. Such standards and tools improve the chances of success in this first foray and, as we all know, early successes breed confidence and increase commitment.

Service architectures should use [OAuth2](https://tools.ietf.org/html/rfc6749). It is secure, widely implemented, and well supported in most technology stacks. The auth component will perform the authorization server role. All other components will be resource servers and/or clients.

The authorization request & grant portion of the OAuth2 flow should be short circuited for internal components. There is no need to ask the user to explicitly grant authorization to a mere implementation detail of the overall system. Such a short circuiting is usually accomplished by keeping track of which clients are internal components. When an internal component requests authorization the authorization server simply grants it immediately (after authenticating the user, of course).

The combination of simplicity and existing standards make auth a great first service to extract.

[^capability-based-authorization]: [Capability based authorization](https://en.wikipedia.org/wiki/Capability-based_security) is my preferred scheme, if it can be applied easily. In practice capability based authorization usually means generating unguessable URLs for individual resources. Simply knowing the URL implies that you are authorized to access the resource. Think of it like a door lock: if you have the key you get in, the lock doesn't know anything about who should have access, or why. Such an approach reduces the inter-component communication and improves performance. This makes it a highly attractive option, however, it is not always practical.

## New features

Once you have extracted authorization and authentication it is time to take this show on the road. My preferred second target is a totally new feature. Building a new feature as a service has several advantages. First, new features are inherently less coupled to the existing code. This makes them easier to implement in a separate component. Second, new features are usually lower risk politically. Changing the way an existing feature works will usually raise some concerns, but fewer people have a vested interest in hypothetical features. Third, it sets a precedent to build on later. If new features *can* be implemented outside the monolith then it can become policy that new feature *are always* implemented outside the monolith. In this way we can effectively slow the monolith's growth.

Implementing a new feature outside the monolith may require exposing existing capabilities of the monolith as a service. This is okay. As with most refactoring, [we will sometimes have to make things worse before we can make them better](http://softwareengineering.stackexchange.com/a/220261). In this situation, implement a service in the monolith to expose the necessary functionality and use it from the new component.

Do *not* under *any circumstances* let the new component use the same database tables[^views] as the monolith. Doing so will only couple the two codebases in a that will both harder to maintain.

[^views]: In some cases views can be used to define a maintainable interface between the monolith's database and new components. Specifically, the monolith would define views in a new schema that was specifically for external components to access. These views need unit tests, etc, to ensure they are maintained as the monolith's datamodel evolves. In general, this is more trouble than it is worth, and sets the wrong precedent to boot, but there are times when throughput, bandwidth, or latency concerns make it a reasonable choice.

## Existing seams

Once auth is a separate component and at lease one new feature has been implemented outside the monolith we can extract some functionality. The key is to understand the existing code base enough to see seams in it that could be used to cleave off a bit of functionality. This functionality will likely the best factored and designed part of the monolith. It is ironic that the easiest part to remove is the best part, but c'est la vie.

In most "mature programming environments" there will be multiple plausible candidates for extraction. It is important to have a list of candidates ready. Begin by taking every opportunity to tighten the encapsulation and reduce the coupling of extraction candidates. Every bit of this sort of refactoring done  ahead of time improves the chances of successful extraction. Even if the section of code is never extracted, this work improves the maintainability of the code, so it is useful regardless.

Chance favors the prepared, as they say. Once you have the hit list, every feature request should be viewed as potential extraction opportunity. When possible, expand the scope of feature requests to include component extract before implementation of new functionality. Feature requests that allow for such expansion are rare so don't become discouraged if you don't run across one right away. It will happen, but only if you are constantly vigilant.

Once you have built a consensus around extraction as part of a larger feature, begin by further tightening the encapsulation of the section to be extracted. Once the interface is solid, re-implement the feature in a new component. Once that component is functional, replace the implementation in the monolith with a client to the new component that provides the same interface as the original implementation.

Basically, component extraction is just normal, every day incremental refactoring, but in the large.

## Conclusion



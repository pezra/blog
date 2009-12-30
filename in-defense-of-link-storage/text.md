It seems that more and more are beginning to grasp the
[hypermedia constraint][mark-baker-hypermedia] of REST.  This is an
unmitigated Good Thing.  However, once you get hypermedia the idea of
a client persisting links that it has found starts to seem a little
odd.  For example, Kirk Wylie describes clients that store links as
"not well behaved" in his
[excellent presentation on REST in financial systems][rest-financial-systems-video].
Even on the rest-discuss mailing list there is
[no consensus on the matter][link-storage-thread].

[mark-baker-hypermedia]: http://www.infoq.com/articles/mark-baker-hypermedia
[link-storage-thread]: http://tech.groups.yahoo.com/group/rest-discuss/message/13516
[rest-financial-systems-video]: http://www.infoq.com/presentations/restful-financial-systems-integration

The idea of an application as a set of states (read: representations)
with transitions (read: links) to other states seems to go against the
idea of storing links.  Transitions from one application state to
another are surely transient.  Any change in the application state,
either by this client or some completely unrelated client, could
easily invalidate those transitions.  In that context a client that
stored links for later use would surely be doomed dereferencing dead
links for the rest of it's days.

Further, the idea that clients might store links is a frightening
specter for maintainers of services.  If clients store links, and you
prefer not to break those clients, you must continue supporting any
links you have ever included in any representation in perpetuity.
Talk about limiting your design freedom.  Such a strict requirement
would surely raise the cost of maintaining the service over time.

Reality sets in
-----------

Those are scary thoughts.  Some of these issues are even real.  But
end the end it doesn't matter.  Almost all non-trivial systems are
going to require that URIs be stored in places other than the origin
server.  Sometimes these stored URIs will merely be caches.  Other
times they will be data that cannot be recalculated mechanically.

For example, say you have an order taking system and an inventory
system.  When placing an order the user goes to the web site, searches
for "coffee", selects the third item in the results and places an
order 1 of that item.  An order is a set of line items each of which
references a product.  Once payment is received the order system is
going to need to be able to tell the shipping department which items
from inventory to send to the customer.

The inventory system has, of course, a URI for every type of product
that is for sale.  So the simplest and most effective way for an order
to reference a product is to use that the inventory URI for that
product.  URIs are called universal resource *identifiers* for a
reason, we might as well use them as such.

In this example, we have a situation where the product references in
the order are not merely caches of URIs.  Many things may change the
ordering of search results -- a new product being added, an old one
being discontinued, even a small change to a description of some
product.  So at any moment the the third item in the search results
for "coffee" might be different.  Once the user has made their
selection no automata can reliably retrace those steps.

The implications of this are significant.  The inventory must continue
to support the product URIs used in orders until such time as the
order system would never care to dereference those URIs again.  If a
month from now the user comes back and wants to see their order
history, those product URIs had better still work.

Fortunately, HTTP provides us with a ready solution.  Behold the
awesomeness that is [HTTP redirection][moved-permanently].  HTTP
redirection is your best friend when it comes to gracefully changing
REST/HTTP applications.  Clients get what they need -- URIs continue
to work as identifiers indefinitely -- and servers get what they need
-- a lot of freedom to change the names and dispositions of resources.

[moved-permanently]: http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.3.2

Transient links
---------------

We are still faced with this issue of the transient nature of links.
Certainly, many links encode transitions which may be transient.  The
client has no general way of distinguishing between links which
represent transiently available state transitions, and those that
represent more permanent transitions.

In our example, immediately after creating an order, it probably
provides some links to pay for the order.  After the user has provided
payment those transition would no longer be valid.  However, the link
to the inventory product is a more permanent part of the order
resource.

The only tractable way i see to deal with this issue is to document
the lifespan the various link found in a representation.  Once the
client implementer understand the semantics of the links they well
often be able to infer the likely lifespan of the links without
further input.  However, guidance can be provided in situations where
precision is required or the lifespan is ambiguous.  A transient link
is, by definition, an option part of the representation so documenting
the conditions that cause it to be present is likely to be required
anyway.

Best practices: Server
---------

REST/HTTP application developers should assume that clients will store
links and dereference them after indeterminate periods of time.  When
resources are relocated or renamed requests to the resource's obsolete
URI should be redirected to the canonical URI using a `301 Moved
Permanently` response.

For links whose validity has a bounded lifespan the documentation of
the representations (the media type) should explicitly layout that the
link is transient and optional.  If possible the documentation should
also describe the conditions of the links existence.

Remind client developers early and often that client *must* follow any
and all redirects from the server.

Best practices: Client
---------

Clients should follow redirects.  Fastidiously.

Clients should update it's internal storage upon receiving a `301
Moved Permanently` response by replace the URI it requested with newly
provided location.

Client developers should be aware of transient links in the
representations being dealt with.  Either do not store these URIs or
ensure that attempts so use these URIs handle failure in ways that
make sense for the application.

Believe and follow the redirections the server sends to you.
Seriously.



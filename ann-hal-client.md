[HalClient][] is yet another ruby client library for [HAL][] based web
APIs. The goal is to provide an easy to use set of abstractions on top
of HAL without completely hiding the HAL based API underneath. The
areas of complication that HalClient seeks to simplify are

 * [CURIE][] links
 * regular vs embedded links
 * templated links
 * working [RFC6573][] collections

Unlike many other ruby HAL libraries HalClient does not attempt to
abstract HAL away in favor of domain objects. Domain objects are great but
HalClient leaves that to the application code.

CURIEs
---

CURIEd links are often misunderstood by new users of HAL. Dealing with
them is not hard but it requires care to do correctly. Failure to
implement CURIE support correctly will result in future breakage as
services make minor syntactic changes to how they encode
links. HalClient's approach is to treat CURIEs as a purely
over-the-wire encoding choice. Looking up links in HalClient is always
done using the full link relation. This insulates clients from future
changes by the server to the namespaces in the HAL representations.

Links are links
---

From the client perspective there is very little difference between
embedded resources and remote links. The *only* difference is that
dereferencing a remote link will take a lot longer. Servers are
allowed to move links from the `_links` section to the `_embedded`
section with impunity. Servers are also allow to put half of the
targets of a particular rel in the `_links` section and the other half
in the `_embedded` section. These choices are all semantically
equivalent and therefor should not effect clients ability to function.

HalClient facilitates this by providing a single way to navigate
links. The `#related(rel)` method provides a set of representations
for all the resources linked via the specified relationship regardless
of which section the links are specified. Clients don't have to worry
about the details or what idiosyncratic choices the server may be
making today.

Templated links
---

Templated links are a powerful feature of HAL but they can be a little
challenging to work with in a uniform way. HalClient's philosophy is
that the template itself is rarely of interest. Therefore the
`#related` method takes, as a second argument, as set of option with
which to expand the template. The resulting full URL is used
instantiate a new representation. This removes the burden of template
management from the client and allows clients to treat templated links
very similarly to normal links.

[RFC6573][] collections
---

Collections are a part of almost every application. HalClient provides
built in support for collections implemented using the standard
`item`, `next`, `prev` link relationships. The result is a Ruby
`Enumerable` that can used just like your favor collections. The
collection is lazily evaluated so it can be used even for very large
collections.

Conclusion
---

If you are using HAL based web APIs I strongly encourage you to use a
client library of some sort. The amount of resilience you will gain,
and the amount of minutiae you will save yourself from will be well worth
it. The Ruby community has a nice suite of HAL libraries whose level
of abstraction ranges from ActiveRecord style ORM to very thin veneers
over JSON parsing. HalClient tries to be somewhere in the
middle. Exposing the API as designed while providing helpful commonly
needed functionality to make the client application simpler and easier
to implement and understand.

HalClient is under active development so expect to see even more
functionality over time. Feedback and pull requests are, of course,
greatly desired. We'd love to have your help and insight.




[comverge]: http://comverge.com
[hal]: http://stateless.co/hal_specification.html
[halclient]: http://github.com/pezra/hal-client
[CURIE]: http://www.w3.org/TR/curie/
[RFC6573]: https://tools.ietf.org/html/rfc6573
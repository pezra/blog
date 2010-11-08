When designing a information service one decision that must be made is
what sort of media type to use.  Choices range from a custom XML
flavor, to [JSON][], to [HTML][].  Each of these choices have their pros an
cons.

## Custom XML

Designing a service specific format using XML is probably the most
common approach.  This approach provides a high level of clarity in
data representations.  However, it has very limited reach.  Only
clients specifically designed to understand this format will be able
to understand these representations.  For more services this means
that very few client will be able to work with these representations.

Also XML representations tend to be quite verbose.  Most bandwidth
concerns can be overcome with compressions.  The use of HTTP
[transfer-encoding][] is a particularly easy way to implement such
compression.  However, XML is quite complex to parse.  The size and
complexity of XML representations can impose a non-trivial cost on
clients.

## Custom JSON

JSON representations have become quite common recently due to the
ubiquitous use of [AJAX][].  JSON is a data serialization language
based on the Javascript language.  JSON representations are generally
simpler and smaller than equivalent XML representations.

JSON provides a more well defined set of semantics than XML.  This
makes parsing it simpler, and requires fewer resources, than XML.  It
also makes it somewhat less expressive for complex entities.  Rather
than defining exactly the structure is needed, entities must be
mapped into the provided native data structures.

## Atom

[Atom][] is an XML based representation format originally designed as
a way to syndicate blog posts.  Atom is extensible and can be used to
publish feeds of any sort of data.  The standardized nature of Atom
increases it's reach, i.e. there are may more clients that can
understand Atom documents.  However, most Atom feeds are for HTML
items therefore Atom clients work best with HTML entities.

Atom's XML base format also means it inherits the size and resource
parsing issues of custom XML.  However, there are Atom processing
libraries than can be used off-the-shelf to parse Atom documents.


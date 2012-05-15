Threads about how to version hypermedia (or REST) APIs are
multitude. I certainly have made [my opinion
known in the past][rest-versioning]. That being said, the most common approach being
used in the wild is putting a version number in the URI of the
resources which are part of the API. For example,
`http://api.example.com/v1/products/42`.

That approach has the advantage of being simple and easy to
understand. Its main downside is that it makes it difficult for
existing clients to switch to a newer version of the if one becomes
available. The difficultly arises because most existing clients will
have bookmarked certain resources that are needed to accomplish their
goals. Such bookmarks complicate the upgrade quite
significantly. Clients who want to use an upgraded API must choose to
rewrite those bookmarks based on some out of band knowledge, support
both the old and new version of the API, or force the user to start
over from scratch.

None of these are good options. The simplest, most attractive approach
is the first. However, forcing clients to mangle saved URIs reduces
the freedom of the server to evolve. The translation between the two
versions of the API will have to be obvious and simple. That means you
are going to have to preserve key parts of the URI into the new
structure. You cannot switch from a numeric surrogate key to a slug to
improve your SEO. Likewise, cannot move from a slug to a numeric
surrogate key to prevent name collisions. You never know when the
upgrade script will be executed. It could be years from now so you
will also need to maintain those URIs forever. Some clients have
probably bookmarked some resources that you do not think of as entry
points, you will need to be this careful for every resource in your
system.

The second option, forcing clients to support both versions of the
API, is even worse that the first. This means that once a
particular instance of a client has used the API it is permanently
locked into that version of that API. This is horrible because it
means that early users cannot take advantage of new functionality in
the API. It is also means that deprecated versions of the API must be
maintained much longer than would otherwise be necessary.

The third option, forcing users to start over from scratch, is what
client writers must do if they want to use functionality which is not
available in the obsolete version when there is no clear upgrade path
between API versions. This is not much work for the client or server
implementers but it seriously sucks for the users. Any configuration,
and maybe even previous work, is lost and they are forced to recreate
it.

a way forward
-----

Given that this style of versioning is the most common we need a
solution. The [link header][link-header] provides one possible
solution. We can introduce a link to relate the old and new versions
of logically equivalent resources.  When introducing a breaking API
change the server bumps the API version and changes the URIs in any
way it likes, eg the new URI might be
`http://example.com/v2/products/super-widget`. In the old version of
the API a link header is added to responses to indicated the
equivalent resource in the new API, eg `http://example.com/v2/rels/v2-equivalent`.

    >>>
    GET /v1/orders/42 HTTP/1.1
    ...
    
    <<<
    HTTP/1.1 200 OK
    link: <http://example.com/v2/orders/super-widget>; rel="alternate http://example.com/v2/rels/v2-equivalent"
    ...

Older clients will happily ignore this addition and continue to work
correctly. Newer clients will check every response involving a stored
URI for the presences of such a link and will treat it as a
redirect. That is, they will follow the link and use the most modern
variant they support.

If you are really bad at API design you can stack these links. For
example, the v1 variants might have links to both the v2 and v3
variants. Chaining might also work but it would require clients to, at
least, be aware that any intermediate version upgrade link relations
so that they could follow that chain to the version they prefer.

You could also add links to the obsolescent variant's body. This would
be almost equivalent except that it requires clients to be able to
parse older responses enough to search for the presence of such a
link. Using the HTTP link header field nicely removes that requirement
by moving the link from the arbitrarily formatted body to the HTTP
header which will be supported by all reasonable HTTP clients.

Using URIs to version APIs may not be the cleanest way to implement
versioning but the power of hypermedia allows us to work around its
most obvious deficiencies. This is good given the prevalence of that
approach to versioning.

[link-header]: http://tools.ietf.org/html/rfc5988
[rest-versioning]: http://barelyenough.org/blog/tag/rest-versioning/

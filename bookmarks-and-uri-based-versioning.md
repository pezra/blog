Thread about how to version hypermedia, or REST, APIs are multitude. I
certainly have made my opinion known in the past. That being said, the
most common approach being used in the wild is putting a version
number in the URI of the resources which are part of the API. For
example, `http://api.example.com/v1/orders/42`.

This has approach has the advantage of being simple an easy to
understand. Its main downside is that it makes it difficult for
existing clients to switch to a newer version of the if one becomes
available. The difficultly arises because most existing clients will
have bookmarked certain resources that are needed to accomplish their
goals. Such bookmarks complicated the upgrade quite
significantly. Clients who want to use an upgraded API must choose to
rewrite those bookmarks based on some out of band knowledge, support
both the old and new version of the API, or to force the user to start
over from scratch.

None of these are good options. The simplest, most attractive approach
is the first. Just modify the bookmarked URIs to point to the
equivalent resource in the new version. This wo


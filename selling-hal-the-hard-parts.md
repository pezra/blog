At work we recently embarked on a significant new project. The overall
architecture of this project is a hypermedia service oriented
architecture. We settled on [HAL][] as our basic message
format.[^not-json-ld] The other members on the team have less
experience with both general service oriented architectures and with
hypermedia so a little selling was required to convince them of the
wisdom of this approach. Interestingly the toughest part of this
effort was explaining some of the implementation level design choices
made by HAL. The overall idea of hypermedia based APIs was reasonably
easy for the team to understand and the benefits where not hard to
describe.[^hypermedia-for-internal]

### Segeregated links

My experience is that many developers (and I count myself among this
group) view URLs as, essentially, just pointers to data that an
identity. From this perspective segregating links seems strange. Why
should the fact that a bit of data has an identifier effect its
disposition in the representational? As and example, consider the
humble shipping address. In some systems addresses are not
individually identifiable, they are just a complex property of the
order. In other systems addresses are individually identifiable (for
example, so the user can pick the shipping address from a list of
previously used ones) so they go in the `_links` section. From the
domain perspective there is exactly zero difference between those two,
it is a pure implementation level choice.

I found it particularly hard to sell this part of hal because I don't
really like it either. On the other hand, most of the other formats
out make similar a choice so it is either accept this or roll your own
and I am thoroughly unconvinced that the world needs yet another JSON
hyper media format.

### Embedded section

The `_embedded` section is also a point of some confusion. The need to
reduce HTTP requests is easy to grasp but it seems to many of the
uninitiated to be a rather complicated way to solve the
problem. However, once links are segregated this, or something
similar, may be the best solution. I have found that the embedded
always requires discussion but gets relatively little push back once
it's purpose is reasonably well articulated.

### URIs for custom relations

The requirement to use URIs for custom relations often meets
significant resistance. Complaints usually center around the
"ugliness" of URIs or how hard they are to read. More user experience
oriented participants will be concerned that the use of URIs for rels
will be off putting for new client developers and will result is
slower or reduced adoption of the API by client developers. This is a
very real concern and one that should not be taken lightly. CURIEs
offer a solution of sorts to the ugly rels problem. With CURIEs a rel
such as `http://example.com/rels/orders` can be represented as
`app:orders`. This prettifies the link rels as the expense of quite a
bit of complexity in both the HAL documents and client code. This
trade off -- beauty for complexity -- may be worth it for APIs that
need to drive rapid adoption.

It is worth noting that most client developers will probably use a
client library of some sort. This means they will not be directly
exposed to the JSON after the initial exploration and API selection
phase of their projects. Of course, those are absolutely critical
periods for the success of most APIs.

Sometimes the aesthetic assessments morph into concerns about how
using URIs as relations "complicates" the format. I don't buy that the
beauty, or lack thereof, effects the complexity in any way. Such
complaints can usually be dealt with by clarifying what the
complainant means by complexity in this situation. Once they are
making, essentially, an aesthetic argument discussion tend to revert
to the issues discussed above.


[^not-json-ld]: I seriously considered [JSON-LD][] and in many ways I
like it but think that it is a little too far removed from its
semantic model. To safely consume a JSON-LD API you need a lot RDF
machinery and at that point I think [Turtle][] would be a better
choice.

[^hypermedia-for-internal]: I believe in the power of hypermedia APIs
even when the clients of that API are likely to be are written by the
same organization as the servers. Clean, well defined interfaces are a
bit more work they are a good thing even if you are the person working
on both sides of the interface.

[JSON-LD]:
[Turtle]:
[HAL]:
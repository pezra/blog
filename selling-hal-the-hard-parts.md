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
order. In other systems addresses are individually identifiable (so
the user can pick the shipping address from a list of previously used
ones) so they go in the `_links` section. From the domain perspective
there is exactly zero difference between those two, it is a pure
implementation level choice.

I found it particularly hard to sell this part of hal because I don't
really like it either. On the other hand, most of the other formats
out make similar a choice so it is either accept this or roll your
own. I am thoroughly unconvinced that the world needs yet another JSON
hyper media format.

### Embedded section

### URIs for besoke relations





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
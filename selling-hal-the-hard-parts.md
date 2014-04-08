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



[^not-json-ld]: I seriously considered [JSON-LD][] and in many ways I
like it but think that it is a little too far removed from its
semantic model. To safely consume a JSON-LD API you need a lot RDF
machinery and at that point I think [Turtle][] would be a better
choice.

[^hypermedia-for-internal]: I believe in the power of hypermedia APIs
even when the intended consumers are written by the same
organization. Clean, well defined interfaces are a bit more work they
are a good thing even if you are the person working on both sides of
the interface.

[JSON-LD]:
[Turtle]:
[HAL]:
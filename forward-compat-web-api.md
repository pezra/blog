# Web API best practices -- forward compatibility

Designing APIs and clients for forward compatibility it the best way to achieve evolvability and maintainability in a microservice architecture. [Wikipedia defines forward compatibility as "a design characteristic that allows a system to accept input intended for a later version of itself."][wikipedia-def] Designing APIs and clients to be forward compatible allows each side of that relationship to change independently of the other. Forward compatibility is an exercise in reducing [connascence][]. We want a system in which each component (service) can modified independent without breaking the current behavior of the system. Without such independence any microservice architecture is doomed to collapse under its own maintenance cost.

Change is inevitable. Any successful architecture must accept, even welcome, change. Building avenues for adaptation into our designs allows our systems to survive and thrive in the face of change. This set of best practices has proven to provide superior forward (and backward) compatibility. As described here these practices are specific to HTTP and web APIs but the general ideas apply to any application protocol.

Forward compatibility is largely the responsibility of clients or document consumers. However, each forward compatible pattern is supported by backwards compatibility in the servers or document consumers. The following list of patterns describes the responsibilities of both parties. If either side fails to uphold their end of the contract compatibility will suffer.

## redirection ##

Clients *must* follow [redirections][redir]. 

URLs change over time. Companies rebrand their products, get acquired, change their name, etc. Developers relocate services to more optimal components. New features sometimes require changes to URLs. Redirection allows servers to change URLs without breaking existing clients by providing runtime discovery of the new URLs.

### supporting backward compatibility ###

A URL, once given to a client, represents a contract. To honor these contract servers *must* provide redirection of existing URLs when they change. Even in hypermedia APIs clients will bookmark URLs. 

## must ignore semantics

Document consumers *must* ignore any features of documents they don't understand.

Document formats change over time. New features require new properties and links. New information illuminates our understanding of cardinalities and datatypes. Ignoring unrecognized document features allow producers to add new features without fear of breaking clients.

### supporting backward compatibility

Document producers *must* not remove, or change the syntax or semantics of existing document features. Once a document feature has been released it is sacrosanct. Idiosyncrasy is better than carnage.

## hypermedia APIs

Clients *must* discover available behavior at runtime.

Available behavior changes over time. We discover new business rules. We realize that existing features are ill conceived. [Hypermedia APIs][] provide a easy mechanism for clients to discover behavior by embedding links in API responses. Clients *must* handle the absence of expected links gracefully. Clients *must not* construct URLs

### supporting backward compatibility

The API *must* be defined using a hypermedia format. The exact format doesn't matter. ([HAL][], [JSON API][], [Hydra][], etc are all good.) What does matter is that all behaviour of the API *must* be discoverable at runtime.


## server content negotiation

Document formats have a half life. At some point the idiosyncrasy build to a the point that a new document format is worth the effort. Sometimes you realize that a particular representation is too bulky or too terse.

Clients *must* inform the server of what document formats they can process via the [`Accept` header][accept]. This allows the server to introduce new, improved representations of existing resources while continuing to serve existing clients.

### supporting backward compatibility

Servers *must* continue honoring requests for existing document formats.


[accept]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Accept
[json api]: http://jsonapi.org/
[hydra]: https://www.markus-lanthaler.com/hydra/
[hal]: http://stateless.co/hal_specification.html
[hypermedia api]: http://apievangelist.com/2014/01/07/what-is-a-hypermedia-api/
[redir]: https://developer.mozilla.org/en-US/docs/Web/HTTP/Redirections
[connascence]: https://en.wikipedia.org/wiki/Connascence_(computer_programming)
[wikipedia-def]: https://en.wikipedia.org/wiki/Forward_compatibility

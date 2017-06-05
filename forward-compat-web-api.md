# Web API best practices -- forward compatibility

Designing APIs and clients for forward compatibility it the best way to achieve evolvability and maintainability in and microservice architecture. [Wikipedia defines forward compatibility as "a design characteristic that allows a system to accept input intended for a later version of itself."][wikipedia-def] Designing APIs and clients to be forward compatible allows each side of that relationship to change independent of the other. As with all software design, forward compatibility is an exercise in reducing [connascence][]. We want a system in which each component (service) can modified independent without breaking the current behavior of the system. Without such independence any microservice architecture is doomed to collapse under its own maintenance cost.

Change is. Period. Any attempt to stop change will fail. The only hope for success is welcome change and build avenues for adaptation into our designs. This set of best practices have proven to provide superior forward (and backward) compatibility. As described here these practices are specific to HTTP and web APIs but the general ideas apply to any application protocol.

Forward compatibility is largely the responsibility of clients and document consumers. However, each forward compatible pattern is supported by an backward compatibility. The following list of patterns describes the responsibilities of both parties. If either side fails to uphold their end of the contract there can be no compatibility.

## redirection

URLs change over time. Companies rebrand their products, get acquired, change their name, etc. Developers implement services in suboptimal components and later those services are relocated. Sometimes new features require changes to URLs.

Fortunately, [HTTP provides an excellent mechanism for handling such changes: redirection][redir]. All clients *must* follow redirections. Failing to do so is nothing more, or less, than an undiscovered bug. Clients that scrupulously follow redirections are forward compatible and will not break when URLs change.

### supporting backward compatibility

To support this forward compatibility servers *must* provide redirections when URLs change. Failing to do so will cause widespread failure.

## must ignore semantics

Document formats change over time. New features require new properties and links. New information illuminates our understanding of cardinalities and datatypes.

Document consumers *must* ignore any features of documents they don't understand. This allows document producers to add new features without fear of breaking clients.

### supporting backward compatibility

Document producers *must* not remove, or change the syntax or semantics of existing document features. Once a document feature has been released it is sacrosanct. Idiosyncrasy is better than carnage.

## hypermedia APIs

Available behavior changes over time. We discover new business rules. We realize that existing features are ill conceived.

Clients *must* discover behavior at runtime. [Hypermedia APIs][] provide a easy mechanism for clients to discover behavior by embedding links in API responses. Clients *must* handle the absence of expected links gracefully. Clients *must not* construct URLs

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

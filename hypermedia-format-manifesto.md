There has been a fair bit of discussion in [HTTP APIs hypermedia channel](https://httpapis.slack.com/messages/C1JQJUF2T) lately about hypermedia formats (particularly those of the JSON variety). Personally, i find all of the existing options wanting. I'm not sure the world needs yet another JSON based hypermedia format but the discussion did prompt me to try to articulate what i value in a format. I have organized this in a value comparison format similar to the [agile manifesto](http://agilemanifesto.org/). I value the things on the right, but i value the things on the left more.

## values

Through my work developing and consuming APIs i have come to value:

- **evolvability** over message and implementation simplicity
- **self describing messages** over reduced message sizes
- **standards** over bespoke solutions
- **human readability** over client simplicity
- **uniformity** over flexibility

## evolvability over message and implementation simplicity

APIs and the producers and consumers of APIs must be able to evolve over time. Evolvability inherently means that the message and implementations will be more complex. Designers and implementers must have forward compatibility in mind at all times. This forward compatibility mindset produces features that add value only after, months, years or even decades of life. Having those features is more complex than not, but the return on those investments is worth the cost.

## self describing messages over reduced message sizes

Embedding all the information needed to interpret a message simplifies client implementation and improves evolvability. However, embedding all that information necessarily increase the size of the message. For most APIs the additional data transfer is just not important enough to give up the benefits of self describing messages.

## standards over bespoke solutions

Standards allow reuse of code and of knowledge. Standards often encode hard won practical experience about what works and what doesn't. However, standard solutions often don't fit as well as purpose designed solutions to specific problems.

## human readability over client simplicity

It is important that APIs the understandable by mere mortals. An average developer with [`curl`](https://curl.haxx.se/) should be able to interact with the API and understand it. Achieving human readability while also honoring the other values often means that clients must become more complicated.

## uniformity over flexibility

There should be a small number of ways to express a particular message. This makes consumer and producer implementations simpler. However, this means that existing APIs will likely be non-conformant. It also means that some messages will be less intuitive and human readable.

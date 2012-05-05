One of the hardest won lessons of my career is the power of incrementalism. I am, at my core, an idealist. When i look at a product or code base i tend to see the ways it deviates from my ideals, rather than the ways it is useful or does match my ideals. 

This tendency is a powerful motivator to produce elegant code and good products. I suspect it is one of the reasons i have become a good software developer. It does have its dark side though. Overcoming some of the weaknesses of my idealism is an ongoing theme in my life. Most importantly idealism makes developing a sense of "good enough" very difficult. 

Getting a intuitive feel of what constitutes "good enough" is probably one of the most important skills any engineer can develop. An almost sure sign of an underdeveloped sense of good enough is the big rewrite. Early in my career i was involved in a couple of big rewrites. Both were successful, at least from a technical perspective, but both were a complete failure from business perspective. They took too long and did not provide any immediate value to the customer.

In both cases i was crucial in convincing the business to do the big rewrite.

In the intervening years i have come to realize that, to use a football analogy, real progress is made with 4 yards and a cloud of dust. If you want a successful product, or a nice code base you will get it one small improvement at a time. The bigger the change you are contemplating the more uncomfortable you should be. If it is going to take more than a few man weeks of effort there is almost certainly a better, more incremental, way to achieve the goal.

I am not saying you should not rewrite the product(s) you work on. Quite the contrary actually. If you are not in the middle of completely rewriting your code base you are Doing It Wrong. But that rewrite should be broken into many small improvements and it should never end. Small refactors improving what ever code you happen to be looking at today. Those improvements will, over time, constitute a complete rewrite in practical terms. However, the business and customers will continue to get value out of the improvements as the are made, rather than having to wait a year or more. And the developer benefit too, because the changes get released and battle tested in smaller, easier to debug pieces.

A strategic vision of what the end state looks like is vital for any rewrite. A continuous incremental rewrite is no different. You need to have a feel for where you want to be in 1-3 years. Some refactoring will be simple coding improvements but most of the time you want to combine simple coding improvements with architectural improvements that will leave space for anticipated features. 

[Signalling] is very important for the continuous incremental rewrite, both in the code and in the product. Part of this discipline is leaving space for anticipated improvements but not actually implementing them until there is sufficient evidence that is it worth the effort. To collected such evidence you have to suggest to the user or reader that certain improvements are possible. Once the user or reader is aware that such directions are possible they are much more likely to push for the ones they will find valuable. This signalling should usually be fairly subtle. The best way is to build the interface so that it suggests a mental model of the system that flexible enough to support additional functionality that you suspect may be useful. 

If you are not able to that subtle in the interfaces, and it is often very difficult, just changing expanding your own mental model to allow more flexibility is a good start. How you think about the problems will invariably effect the interfaces you design. Your internal models will leak into the design, so make sure they are good ones.




[Signalling]: http://en.wikipedia.org/wiki/Signalling_(economics)

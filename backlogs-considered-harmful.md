There are somethings we do under the pretense of being useful that are
actually harmful.  Unscheduled stories and bug reports in your ticket
tracking system, are one example.

Creating a ticket is easy when you are "in the moment".  However, one
produced these artifacts have to be read and understood multiple times
in the future.  Each time you touch or read a ticket it costs time.
How many times have you given up trying to find some ticket that you
think you wrote a long time ago and just entered a new one?  How much
time have you spent sifting through the same tickets every iteration
deciding repeatedly that they are not important enough to actually
schedule?

To be clear i am not saying that you should not enter tickets in your
issue tracker.  What i am saying is that doing so is *not* free.
Therefore, you should consider very carefully whether the story or bug
you are about to write will have a net positive value over the life of
the project.  Most likely it will not.

My rule of thumb is this: do not write it down unless you are willing
to schedule it right now.

Willingness to schedule a bit of work is proxy for it's importance.
It is easy to pretend everything is top priority, but if you are not
willing prioritize a bit of work before something you have already
decided you want it is obviously not very important.

### If it important you will not forget

I think the idea that you will forget something important is the
biggest fear related to this approach.  That fear is just silly.  If
you are passionate about an idea you will not forget it.  If it
important to your customers they will not let you forget it.

Let me ask you a question: if neither you nor the customers care
enough about an issue to get it on the schedule should be you
expending effort on it?

To me the answer is clearly no.  If you see a potential issue or have
an idea let it rest until it become important.  Odds are it never will
become important and you will have saved a good deal of effort.  If it
ever does become important the fact that six months ago you wrote a
ticket vaguely related to issue people are having it six months ago
will not help anyway.  You probably will not even be able to find it.

### Corollary: todo comments considered especially harmful

Notice that my rule of thumb basically rules out todo comments
altogether.  Every todo comment is not only an unscheduled story, but
an unscheduleable story.  Even if the story where in the ticketing
system it would probably never, ever be scheduled.  If it had a chance
the developer would have probably written story instead.

In addition to being Todo comments are a way for the developer to transfer some of the weight of
the decisions that where made to future generations.  A tax, in
effect, on future generations of developers in order to assuage the
author's insecurities regarding decisions they have made in the code.

To the authors of todo comments i say, own your decisions.  If you are
not sure of what to do get a second pair of eyes now.  Don't burden
future developers with your indecision.  Right or wrong it will work
out better if you make a reasonable decision and own it until there is
some evidence that it was wrong.  If you where wrong, that evidence
will come in the form of a bug report


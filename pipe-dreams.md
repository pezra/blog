As you know, i am [on a job hunt][seeking-work].  Like everyone else i
use a variety of sources to find job postings.  I also have a wide
breadth of skills so i also search on a lot of keywords.  Going to
5-10 websites and doing 2 to 20 searches each gets old really fast.
Worse yet some of these sites don't really have particularly
sophisticated search capabilities so you end up having to look at a
bunch of posts that are not really relevant.

So, over the weekend i decided to use [Yahoo Pipes][pipes] to
aggregate and filter all the job posting feeds.  Pipes is really
impressive, at first.  Creating a basic version of the pipe i wanted
was surprising easy.  There was definitely some learning to be done
but i had a working prototype up in very short order.  

Once i started expanding from the initial prototype the dream started
collapsing around me.  My pipe had a fairly complex set of filtering
rules that i wanted to apply to each of a set of feeds.  My initial
thought was to extract those rules and reuse it multiple pipes, each
handling data from difference sources.  At it turns out, Pipes does
not allow that.  You can use other pipes as the input to a pipe, which
is nice, but the lack of ability to embed other pipes in the middle of
a pipe is a real missed opportunity.  

With that option off the table I decide to create one ginormous pipe
that aggregated a bunch of feed before running their contents through
the filters.  That works, in principle, though it raised some
factoring challenges in my case.  So once i had constructed my very
large pipe I tried to save it, and this is what i got

My initial thought was to extract those rules into a custom operation.
Since that is not supported I instead created a 


 2
groups of heavy logic: feed URI building and item filtering.


And then I
notice the first problem.


[pipes]: http://pipes.yahoo.com
[seeking-work]: http://barelyenough.org/blog/2009/02/need-job/
I have been using [Resque][] quite a bit recently.  It is a really
nice asynchronous job system based on [Redis][].  

Resque checks the queues for jobs to process in a fixed order. (In
alphabetic order, to be precise.)  This turns out to be a problem is
you want predictable handling time for jobs.  For example, consider a
system which has queues `aaa` and `zzz`.  If you add 100 jobs to `aaa`
and 1 job to `zzz`, the job on `zzz` will wait a long time before
being processed.

This problem is easily solved by just checkout the queues in random
order.  Over time, any particular queue will be checked early so a few
deep queues will not starve the other queues in the system.

[resque-fairly][] is a Resque [plugin][resque-plugins] which provides
that behavior.  Just install the gem, add `require 'resque-fairly'`
and Resque will handle queues with approximate fairness.

[resque-fairly]: http://github.com/pezra/resque-fairly
[resque]: http://github.com/defunkt/resque
[redis]: http://code.google.com/p/redis/
[resque-plugins]: http://wiki.github.com/defunkt/resque/plugins

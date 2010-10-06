I've been developing using asynchronous jobs quite a bit
lately.[^resque-fairly] There is only one reason to do work
asynchronous.  It takes too long to do it synchronously.

Fortunately, it turns out that many of these very large work loads
are [embarrassingly parallel problems][emb-para].  And look, you have
several (dozen) workers just waiting to do your bidding.  It just
makes sense to break up these large blocks of work into many smaller
chunks so that it can be processed in parallel.

Breaking a task up into many small parts comes with some issues. Any
task that takes long enough to run asynchronously is probably going to
take long enough that you need to track its progress.  

Also the problem is probably not completely parallelizable.  Most
problems seem to have a large portion of easily parallelized work
followed by a bit of work that can only happen after all the parallel
work has been complete.

These patterns show up often enough that i have gotten tired of
repeating myself.  Hence was born [resque-multi-step][].
Resque-multi-step provides the functionality described above.  It
provides compound job support for [Resque][] complete with progress
tracking, error handling, and a completely serial finalization
sequence.

Example
-----

Say you want to reindex all the posts in a blog.  However, committing
solr for each post would be excessively slow.  (Trust me, it really is.)

    Resque::Plugins::MultiStepTask.create("reindex-#{blog.name}") do |task|
      blog.posts.each do |post|
        task.add_job ReindexWithoutCommit, post
      end
      
      task.add_finalization_job CommitSolr
    end
      
This reindexs all the posts in parallel.  Any available workers will
pick up a job to reindex a specific blog post.  Once all those reindex
jobs have completed, the finalization job will be executed.

If you have more that one finalization job, they are executed serially
in the order they were added to the task.

Administrivia 
----

If these issues sound familiar give resque-multi-step a try.  It is
available as a gem so installing is just

    gem install resque-multi-step
    
If you want to contribute head on over to the
[github project][resque-multi-step] and hack away.  If you come up
with something useful i'll integrate it post haste.


[emb-para]: http://en.wikipedia.org/wiki/Embarrassingly_parallel

[^resque-fairly]: [resque-fairly][] was one of the first public out
  comes of such work.  The fair scheduling it provides the basis for
  [this effort][resque-multi-step].

[resque-multi-step]: http://github.com/pezra/resque-multi-step
[resque-fairly]: http://github.com/pezra/resque-fairly
[resque]: http://github.com/defunkt/resque

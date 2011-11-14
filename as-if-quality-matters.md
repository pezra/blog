We changed everything while developing CloudSwing.  Our team
structure, the technologies we used, even the processes.  On the whole
the changes have been good, i think.  

During our retrospective for the initial CloudSwing development our QA
guy said that he believe the quality processes we used were working
well.  That is the first time that has ever happened to me in my
entire career.  Usually the QA department feels under staffed and
under the gun.  It made the unreasonably happy to hear our QA guy
think our QA process belongs in the "Good things" column.

We did change the QA process for CloudSwing. In short we decided to
develop the software as if quality actually matters. That means that
we spend time and effort developing the quality assurance plan for
every feature.  By "we" i mean the developers who will implement the
feature.

The core principle of our process is this: the developer owns a
feature from cradle to release. 

It is the developers responsibility to
 * elicit requirements from stakeholders
 * write *testable* acceptance criteria
 * develop automated acceptance tests
 * implement the feature
 * verify all acceptance (included existing ones) pass

It is QA's responsibility to
 * verify the acceptance criteria make sense
 * verify the acceptance criteria is testable
 * verify the acceptance criteria cover all important variations
 * verify the acceptance tests cover all important variations
 * verify all tests pass in a production like environment
 * perform visual inspection in various browsers
 * accept features into the production branch

In practice QA does develop tests.  However, those tend to be
enhancements to tests written by the developer or tests to cover edge
case that the developer missed.  The upside to this approach is that
responsibility is clear: it is the developers responsibility to
facilitate the process so that the feature gets deployed. If the tests
don't pass for QA, they just kick it back to the developer.

This is a very developer centric process.  It works for us because we
have a lot more developers than QA. However, that is the norm in my
experience. It is an arrangement that can work very effectively once
you create a process that places quality management at the center,
rather than the end, of the process.
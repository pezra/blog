When we started developing [CloudSwing][cloudswing-ann] we decided to
develop CloudSwing as if quality actually mattered. In the past we
have had -- an all too common -- a sense of disconnection between QA
and development. For CloudSwing we decided that needed to change.

To remove the disconnection we decided that quality should be
considered at every step of the process. We also decided that making
exactly one person responsible for each story would make it less
likely that stuff would fall thought the cracks. We don't have the
luxury of having a product manager, or a large QA team[^1] so we
decided that developer should assume the responsibility for the
stories they implement.

[^1]: We do have an excellent QA guy, though.

Our solution is to make the developer responsible for all aspects of
feature development.  That means that developers have to:

 * elicit requirements from stakeholders
 * write *testable* acceptance criteria
 * develop automated acceptance tests
 * implement the feature
 * verify all acceptance (included pre-existing ones) pass
 * verify that the feature, as implemented, matches the desire of the
   champion
 * deploy changes to production

This has lightened the load on QA to the point that our one QA resource can
keep up with four developers. QA still has a lot of responsibility:

 * verify the acceptance criteria make sense
 * verify the acceptance criteria is testable
 * verify the acceptance criteria cover all important variations of
   the feature
 * verify the acceptance tests cover all important variations
 * verify all tests pass in a production like environment
 * perform visual inspection in various browsers
 * accept features into the production branch

<figure>
  <figcaption>Feature development process</figcaption>
  <img src="/blog/uploads/as-if-quality-matters/flow.png"/>
</figure>

We have been using this version[^many-refinements] of the process for
a while now. I think is has worked really well. In fact, during our
last retrospective our QA guy stated that he thought our quality
management processes belonged in the "Good things" category. I think
that is a first i have ever heard a QA person be so positive about
processes.

[^many-refinements]: It has taken a lot iterative refinements to get to
  this version of the process.

This process is very developer centric. It works for us because we
have a lot more developers than any other flavor of job. It is an
arrangement that can work very effectively once you create a process
that places quality management at the center, rather than the end, of
the process.
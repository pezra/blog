When we started developing [CloudSwing][] we decided to develop
CloudSwing as if quality actually matters. By quality i don't just
mean that the code functions as designed. High quality products also
meet the needs of the business and customers.

[cloudswing]: http://cloudswing.openlogic.com/

 In the past there has been a sense of disconnection between the
business, QA and development parts of our team. (An all too common
situation in my experience.) If quality actually matters there must be
good connections between all the stakeholders and quality must be
considered at every step of the process. We don't have a product
manager, or a large QA team[^good-qa]. That means that if quality is
going to be considered at every step it is going to be because the
developers are doing it, there is no one else at many of the steps.

[^good-qa]: The QA team we do have is super top notch, though.

Our solution is to acknowledge developer responsibility for all
aspects of feature development. That unification reduces the chances
of things being overlooked. It does change the workload of developers,
though. Now developers must:

 * elicit requirements from all stakeholders
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
 * refine acceptance tests
 * verify all tests pass in a production like environment
 * perform visual inspections in various browsers
 * accept features into the production branch

<figure>
  <figcaption markdown="1"><h4>New feature development process[^more-fluid-in-practice]</h4></figcaption>
  <img src="/blog/uploads/as-if-quality-matters/flow.jpg"/>
</figure>

[^more-fluid-in-practice]: In practice the process is more fluid that
  it looks in the diagram. Developers are empowered to do what it
  takes to get the job done. Including not following the process.
  However, not following the process requires an
  [affirmative defense][affirmative-defense] when QA asks "WTF?".
  
  
[affirmative-defense]: http://en.wikipedia.org/wiki/Affirmative_defense

We have been using this version[^many-refinements] of the process for
a while now. I think is has worked really well. In fact, during our
last retrospective our QA guy stated that he thought our quality
management processes belonged in the "Good things" category. I think
that is a first i have ever heard a QA person be so positive about
processes.

[^many-refinements]: It has taken a lot iterative refinements to get to
  this version of the process.

Only time will tell if this approach produces significantly better
results but I am very optimistic. So far it seems to have created a
world of difference.

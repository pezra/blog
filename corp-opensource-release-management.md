Once you start using opensource at your day job you are going to want
to improve it.  A few of these changes may be quite specific and of no
value to the community at large.  However, many improvements are going
to be generally useful and should be contributed back to the
community.

Changes that are generally useful should be contributed back to the
community.  This will help the community and help you.  Every change
to an opensource project you maintain raises the cost of keeping your
modified variant current.  Once a change you need is in the mainline
project your company no longer has to maintain it alone.

However, waiting for a change you've made to be released by the
opensource project before putting it in production is probably not
going to be an option.  What ever problem caused you to make the
change needs solving and soon.  It could take weeks or months before
even a good change is merged in to the mainline of an opensource
project.

Ruby, gems and git
-------

A distributed version control system is key to the approach i
use. [Git][] is my preferred tool, but any DVCS would work.  [GitHub][]
really makes this a lot easier than it would be otherwise.

[git]: http://git-scm.com/
[github]: http://github.com

I work mostly in Ruby so i am going to describe that workflow.  Gems,
particularly with the introduction of the new [rubygems.org][], really
lower the bar for releasing Ruby software.  The low effort required to
do so can really make managing your corporate opensource contributions
easier.  A similar approach could be made to work for other release
mechanisms.

[rubygems.org]: http://rubygems.org

What you will need
------

 * A [GitHub][] account for yourself
 * A [GitHub][] account for your company
 * A [rubygems.org][] account for yourself

Making changes
-----

Before getting started on the actual change there are some setup step
you need to perform.  These only need to be done once per opensource
project.

 1. Fork the canonical repo of the opensource project into your Github account.
 2. Fork the canonical repo of the opensource project into your
 companies GitHub account.
 3. Clone your fork onto your machine
       $ git clone {private URI of your repo}
 4. Add an 'foocorp' remote to you local repo pointing to your
 companies fork of the opensource project.
       $ git remote add foocorp {private URI of your companies repo on github}
 5. Create a 'foocorp-stable' branch in your local repo.
       $ git checkout -b 'foocorp-stable'
 6. Push 'foocorp-stable' branch to your companies repo.
       $ git push foocorp foocorp-stable
 7. On the 'foocorp-stable' branch, change the name of the gem to 'foocorp-projname'.
       $ (edit gemspec or gemspec generator)
       $ git commit -m "Company specific Gem name"
 8. Push that change to your companies GitHub account.
       $ git push foocorp
 
So this means you have created a version of this project whose gem has
your companies name prepended.  This will be useful later as a way to
release the changes you need before they have been integrated into the
opensource project.  However, this change is only your companies
stable branch.  This branch will never be integrated into the
opensource project.

Now on to making the change.
 
 1. Create a feature branch for your change in you local repo.
        $ git checkout -b 'super-feature'
 2. Implement the wicked new feature/fix the bug.
        $ (do work)
        $ git commit -m "feature is super"
 3. Push the feature branch to your GitHub repo.
        $ git push
 4. Push the feature branch to your companies GitHub repo.
        $ git push foocorp super-feature
 5. Merge your feature branch into the 'foocorp-stable' branch.
        $ git checkout foocorp-stable
        $ git merge super-feature
 6. Push 'foocorp-stable' branch to you companies GitHub repo.
        $ git push
 6. Bump the version number as [appropriate][rational-versioning].
 7. Build the gem from the 'foocorp-stable' branch.
        $ rake build
 8. Push the gem to [rubygems.org][].
        $ gem push pkg/{gem file}
 9. Change your application to require the 'foocorp-projname' gem
    instead of 'projname'.
 10. Send pull request to the opensource project for you feature branch.
  
[rational-versioning]: http://docs.rubygems.org/read/chapter/7

The end result is that you have a published gem with the changes you
need to support you application.  It has a name that will keep it from
being confused with the original.  The changes you implemented are
available to the opensource project for the benefit of the community
at large.

Once your changes have been integrated into the opensource project and
released you can revert your application to depend on the canonical
version rather than your custom version.

Questions
-----

#### Why use two separate repos on [GitHub][] for you and your company?

Your companies GitHub account will probably have it's email address
setup to point to a distribution list.  Getting a change integrated
into an opensource project can take some back and forth.  By default,
responses to a pull request go to the email of the account that sent
to pull request.  This means that your whole team will be getting
these emails.  As the original author of the change it is your
responsibility to shepherd it through the integration.  Preferably
without barraging the rest of your team with emails they don't care
about.

#### What about non-generic changes?

Follow the same process above except don't send the pull request.  If
you ever want changes from the mainline opensource project you will
need to merge those into your companies stable branch explicitly.
However, this is pretty easy to do with [Git][].

#### What if the opensource project adds a change i want before my changes are integrated?

Just merge the opensource projects release tag (or any commit-ish for
that matter) containing the change you want into your companies stable
branch.  You can do this as many times as needed.

#### Why not modify the version of the gem rather than the name?

You could distinguish your custom gem by appending suffix to the
version.  For example, '1.2.3.foocorp'.  By doing so you would not be
able to push the gems to [rubygems.org][] because someone else already
owns that gem.  Also, if you needed to make multiple changes you would
lose the ability to effectively version you changes.

Conclusion
-----

Using the technique described above you can manage changes to
opensource projects that are required by your applications highly
effectively.  DVCS allows a much more efficient approach to
maintaining multiple related code streams.  These stream have always
exists but never before has it so easy to manage and maintain them.

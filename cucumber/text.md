I have been working pretty extensively with [Cucumber][] for the last
couple of weeks.  In short, it is killer.  You should be using it.

Having just RSpec/unit tests results in a lot of ugly trade offs
between verifying the design and implementation of the parts (or
units) vs the system as a whole.  Using Cucumber completely absolves
[RSpec][] specs and unit tests of any responsibility for proving the
system works.  That allows you to use RSpec/unit tests as tools to
improve the design, and reliability, of individual parts of the system
without losing confidence in the overall systems ability to function
acceptably.

If you are using [Emacs][] i highly recommend [cucumber.el][].  It has
excellent support for editing [Gherkin][] files (from the original,
and excellent, [cucumber.el][cucumber.el-orig]) and key
bindings to execute scenarios, etc and view the output without ever
having to leave the comfort of Emacs.


[rspec-mode]: http://barelyenough.org/blog/projects/rspec-mode/
[gherkin]: http://wiki.github.com/aslakhellesoy/cucumber/gherkin
[rspec]: http://rspec.info
[emacs]: http://www.gnu.org/software/emacs/
[cucumber]: http://cukes.info
[cucumber.el]: http://github.com/pezra/cucumber.el
[cucumber.el-orig]: http://github.com/michaelklishin/cucumber.el

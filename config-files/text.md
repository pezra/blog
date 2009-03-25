Why do people use [YAML][] for configuration files?  For those who are
unfamiliar, YAML is a nice, easy to read data serialization format.  It
is quite similar to [JSON][].  More limited than XML but a lot simpler
to use if you are just doing data serialization.

Strictly speaking configuration is just data, of course, so you can
use a data serialization language to represent your applications, or
libraries, configuration.  The fact that something can be done does
not make it a good idea.  Particularly if you have a highly readable
Turing strength interpreted language readily available.

In some environments, like Java say, using a data serialization
language for configuration makes a lot of sense.  Creating your own
custom configuration language from scratch is likely to be quite a bit
more trouble that it is worth, unless you have a really complex
configurations to express.

On the other hand, if you are using a dynamic interpreted language why
would you give up all that power, familiarity and ease?  Lets look at
a pretty common configuration: how to connect to the database server.
In Rails it looks sort of like this

    development:
      adapter: mysql
      database: my_db_development
      username: my_app
      password:
      
    test:
      adapter: mysql
      database: my_db_test
      username: my_app
      password:

    production:
      adapter: mysql
      database: my_db_production
      username: my_app
      password:
      host: db1.my_org.invalid
   
That is not to bad.  It does include a fair bit of repetition that
could probably be fixed.  This approach starts getting ugly when you
need to have more dynamic configurations.  For example, RPM type
distros use `/tmp/mysql.sock` as the domain socket for the MySQL
database.  Debian type distros use `/var/run/mysqld/mysqld.sock`.  So
you end up with a config file that looks like this

    development:
      adapter: mysql
      database: my_db_development
      username: my_app
      password:
      socket: <%= File.exist?('var/run/mysqld/mysqld.sock') ? 'var/run/mysqld/mysqld.sock' : '/tmp/mysql.sock' %>
    
    ⋮

You always end up needing non-declarative bits in your configuration.
Everyone realizes this at some point.  So much so that the most common
pattern for YAML configuration files in Ruby is for them to support
[ERB][] (a way to embed Ruby in non-Ruby files).

ERB is a really great technology but using it in this way is a waste.
The most common use of ERB is for generating HTML pages.  The static
parts in plain HTML and the dynamic parts generated using embedded ERB
fragments.  But we configuration we don't have to use a standard
format.  We can bring the full power of the host environment to bear
to solve our configuration problem.  

For simple cases, or situations where you are the only user, you could
just have configuration files be pure Ruby but produce a hash
structure like the YAML+ERB version does.

    database[:development] = {
      :adapter  => :mysql,
      :database => 'my_app_development',
      :username => 'my_app',
      :password => '',
      :socket   => File.exist?('var/run/mysqld/mysqld.sock') ? 'var/run/mysqld/mysqld.sock' : '/tmp/mysql.sock'
    }

    ⋮

That is nothing to write home about but it is superior to the YAML+ERB
version because it is more powerful and extensible and it takes even
less code to implement.

I generally prefer to create a small DSL for the configuration.  The
results can be much more pleasant and DRY than the hash oriented
approaches.  Consider this alternative

    database {
      adapter   = mysql
      database  = 'my_db_#{RAILS_ENV}'
      user_name = 'my_app'
      password  = ''
      socket    = File.exist?('var/run/mysqld/mysqld.sock') ? 'var/run/mysqld/mysqld.sock' : '/tmp/mysql.sock'

      env('production') {
        host = 'db1.my_org.invalid'
      }
    }

This is a lot clearer, simpler and less repetitive.  In addition, the
users can do anything they need to because they have access to the
full power of the host language.  This DSL would be a little more
difficult to implement that the YAML+ERB approach, but not much.  For
that additional bit of effort you get a huge improvement in usability
and power.  Your users are worth that effort.


[erb]: http://www.ruby-doc.org/stdlib/libdoc/erb/rdoc/classes/ERB.html
[yaml]: http://www.yaml.org/ 
[json]: http://json.org/


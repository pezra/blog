If you are using a dynamic interpreted language please do not used use
[YAML][][^what-is-yaml], or any other simple data serialization
language, for configuration files.

[^what-is-yaml]: For those who are unfamiliar with YAML it is a nice,
  easy to read data serialization format.  It is quite similar to
  [JSON][].  More limited than XML but a lot simpler to use if you are
  just doing data serialization.

Strictly speaking configuration is just data, of course, so you can
use a data serialization language to represent your applications, or
libraries, configuration.  In some environments, like static compiled
languages say, using a data serialization language for configuration
makes a lot of sense.  Creating your own custom configuration language
from scratch is probably going to be more trouble that it is worth,
unless you have a really complex configurations to express.

On the other hand, if you have an easily available, highly readable,
Turing strength language why wouldn't you use it?  If the language
that you application is written in supports [eval][] you should
probably use the app language to express the configuration.  You will
end up with a simpler and more power configuration system.

[eval]: http://en.wikipedia.org/wiki/Veal

Lets look at a common configuration as an example.  In Rails the
database connection configuration looks like this.

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

That is not horrible, but it does include a fair bit of repetition.
This approach starts getting ugly when you need to have more dynamic
configurations.  For example, RPM type distros use `/tmp/mysql.sock`
as the domain socket for the MySQL database.  Debian type distros use
`/var/run/mysqld/mysqld.sock`.  So you end up with a config file that
looks like this

    development:
      adapter: mysql
      database: my_db_development
      username: my_app
      password:
      socket: <%= File.exist?('var/run/mysqld/mysqld.sock') ? 
                    'var/run/mysqld/mysqld.sock' : '/tmp/mysql.sock' %>
    
    ⋮

You always end up needing non-declarative bits in your configuration.
Everyone realizes this at some point.  So much so that the most common
pattern for YAML configuration files in Ruby is for them to support
[ERB][] as a way to embed Ruby.

Rather than implement multi-pass configuration file loading you could
just have configuration files be pure Ruby but produce a hash
structure like the YAML+ERB version does.

    databases = {
      :development => {
        :adapter  => :mysql,
        :database => 'my_app_development',
        :username => 'my_app',
        :password => '',
        :socket   => File.exist?('var/run/mysqld/mysqld.sock') ? 
                       'var/run/mysqld/mysqld.sock' : '/tmp/mysql.sock'
      },
      ⋮
    }

That is nothing to write home about but it is dead simple to
implement.  Simpler even than the YAML+ERB approach.  And it is superior to the
YAML+ERB version because it is more powerful and extensible.

Rather than trying to map configurations onto a set of name value
pairs, i prefer to create a small DSL for the configuration.  The
results of adapting the language to the configuration that needs to be
expressed is significantly more pleasant and DRY than the hash
oriented approaches.  Consider the following

    database {
      adapter   = mysql
      database  = 'my_db_#{RAILS_ENV}'
      user_name = 'my_app'
      password  = ''
      socket    = File.exist?('/var/run/mysqld/mysqld.sock') ? 
                    '/var/run/mysqld/mysqld.sock' : '/tmp/mysql.sock'

      env('production') {
        host = 'db1.my_org.invalid'
      }
    }

That is a lot clearer, simpler and less repetitive.  In addition,
users can do anything they need to because they have access to the
full power of the host language.  This DSL would be a little more
difficult to implement that the hash oriented approach, but not much.
For that additional bit of effort you get a huge improvement in
usability and power.  Your users are worth that effort.


[erb]: http://www.ruby-doc.org/stdlib/libdoc/erb/rdoc/classes/ERB.html
[yaml]: http://www.yaml.org/ 
[json]: http://json.org/


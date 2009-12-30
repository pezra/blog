[Mr Amundsen][mca-json-link-secion] regarding the design of "semantic
machine media types",

> One simple (and proly not completely functional) approach would be
> to adopt a message that has two main sections: links and data.

I really dislike like the idea of sequestering links in a ghetto all
by themselves.  Links are data and we should not treat them any
different than any other data.  I believe that pretty strongly, now
that i think of it.  I don't like [link elements][atom-link] in XML,
either.  Think it might be easiest to show why by a bit of
extrapolation:

    <complexElement rel="entry">
      <string rel="id">234132</string>
      <string rel="displayName">Peter Williams</string>
      <complexElement rel="name">
        <string rel="familyName">Williams</string>
        <string rel="givenName">Peter</string>
      </complexElement>
      <complexElement rel="emails">
        <link rel="email" href="mailto:pezra@barleyenough.org"/>
        <string rel="type">personal</string>
      </complexElement>
    </complexElement>

That is what a [portable contact][] might look like if we treated all
data the way we treat links today.  That look pretty ugly to me, as i
suspect it does to most people.

[portable contact]: http://portablecontacts.net/

The reason it looks ugly it because it is.  This approach places the
least valuable information in the most prominent place.  Doing it for
properties whose values are first class resources does not make it
better.

Perhaps rather that getting our knickers in a knot about the type of
particular properties we could just treat them as normal data, but tag
them as links so that generic tools could gain some visibility.  For
example:

    <entry xmlns:link="http://unobtrusive-generic-linking.org/">
      ...
      <emails>
        <value link:hrefDisposition="content">mailto:pezra@barelyenough.org</value>
        <type>personal</type>
      <emails>
    </entry>
    
This is idea is a lot like [xLink][] but it seems simpler and more expressive to me.

[xlink]: http://www.w3.org/TR/xlink
    
You could expand the idea to JSON with relative ease.

    {...
     "entry": 
       [{...
         "emails" :
           [{"value"    : "mailto:pezra@barelyenough.org",
             "_linkInfo : {"hrefDisposition" : "value"},
             "type"     : "presonal"}]}]}
            
This approach could be extended to support of the full set of data
need to fully support RESTful applications.




    {"customer    : "http://sample.invalid/customers/24",
     "line_items" : 
       [{"self"     : "http://sample.invalid/line-items/42"
         "quantity" : 5
         "product"  : "http://sample.invalid/product/84"}]}


    {"links" : [{"rel"  : "http://rels.sample.invalid/customer",
                 "href" : "http://sample.invalid/customers/24"},
                {"rel"  : "self",
                 "href" : "http://sample.invalid/orders/43"}]
     "line_items" : 
       [{"links" : [{"rel"  : "self"
                     "href" : "http://sample.invalid/line-items/42"},
                    {"rel"  : "http://rels.sample.invalid/product",
                     "href" : "http://sample.invalid/product/9484"}],
          "quantity" : 5}]}
		    
    

[atom-link]: 
[json-schema-g-group]: http://groups.google.com/group/json-schema
[mca-smmt]: http://amundsen.com/blog/archives/1023
[mca-json-link-section]: http://amundsen.com/blog/archives/1023#IDComment49463101
[hyper-schema-link-section]: http://tools.ietf.org/html/draft-zyp-json-schema-01#section-6

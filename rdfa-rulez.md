Using [HTML+RDFa] is it possible to develop data interchange formats
for very complex domains that allow people to consume the information
without specific tools while simultaneously supporting the development
of sophisticated tools that operate on that data.  And that is super
cool!

When designing/developing data interchange formats the tension between
human and machine readability always exists.  Formats that are easy
and efficient for computers to read tend to be rather difficult for
people to understand.  In recent decades the industry has settled on
XML based languages in large part because XML provides a reasonable
balance between those factors.  It can be read and understood by well
trained people with very limited tooling.

On the other hand, XML is complete gibberish to the non-programmer.
If you want a business person to understand the information in an XML
file you must write tools to display the information is a more human
friendly way.

--

As part of my recent work on the
[Software Package Data Exchange][spdx] i have been exploring some
different data serialization approaches.  The basic idea of SPDX is
that people should be able to easily exchange intellectual property
information about software packages.  

The core abstract model is defined in terms of [RDF][].  The use of
RDF means we have several choices how to render the data into files
when the data needs to be interchanged.  We could create a very
compact format that is specific to this domain.  We could use RDF/XML
which, if combined with an XML schema, would allow easy implementation
without requiring a deep understanding of RDF.  We could use HTML+RDFa
which requires more RDF understanding to implement but makes the data
accessible even without SPDX specific tooling.  I suspect we are going
to support all three approaches but it is the RDFa approach that
intrigues me the most.

[spdx]:http://spdx.org/
[rdf]:
[html+rdfa]: 
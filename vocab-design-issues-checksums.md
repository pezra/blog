The [SPDX][] technical team recently encountered and interesting
situation while developing our [RDF][] vocabulary.

[RDF]: http://www.w3.org/RDF/
[SPDX]: http://spdx.org/

The exact scenario was as follows, the information we store about a
file is potentially invalidated with every change to the contents of
that file.  For example, some code might be added that has different
licensing requirements.  Or all the code that has a particular
licensing requirement might be removed. To make sure that the
information in and SPDX file is valid for a particular file it must be
possible to verify that its contents are identical to the contents of
the file that was analyzed.

In SPDX we support verification of file contents by providing message
digests[^checksum] of every file.  However, message digest (hash)
functions come and go.  MD/5 used to be the norm but has fallen out of
favor.  SHA1 is currently very popular but is steadily being replaced
with SHA256 and SHA512.  We definitely need to support more than one
digest algorithm.

[^checksum]: We decided to call this property `spdx:checksum`.  While
this is technically a misnomer it does effectively convey the intent
of the field.

We had three basic choices: 

 * have separate properties for each digest algorithm, each of which would be a sub-property of the `checksum` property
 * define datatypes for each digest algorithm and have single
   `checksum` property
 * define a class for digests that encapsulates all the data and
   have a single `checksum` property
   
For the first option the graph would look like

    <http://zlib.net/zlib-1.2.5.tar.gz#Makefile> a spdx:file;
      spdx:sha1 "1fac389…"^^xs:hexBinary;
      spdx:sha256 "4aa8223…"^^xs:hexBinary.
      
The resulting graphs are simple and self explanatory.  Support for new
digest algorithms is achieved by the addition of one new properties
for each algorithm.

One potential downside is that some tools might not be able to pass
through digests for algorithms the do not understand.  For example,
SPDX provides a tool to translate SPDX RDF data to and from
spreadsheets.  The digests are inserted into the spreadsheet by the
tool looking for the known digest properties and putting that data
into the appropriate column in the spreadsheet.  This means that novel
digest types would be lost in the translation.  This could be avoided
if the tool supported [OWL][] inferencing but it is unlikely we will
implement that in the near future.  I think requiring OWL
inferencing to work properly is a design smell.
     
[owl]: http://www.w3.org/TR/owl-ref/

A graph for the second option would look like

    <http://zlib.net/zlib-1.2.5.tar.gz#Makefile> a spdx:file;
      spdx:checksum "1fac389…"^^spdx:sha1Hex;
      spdx:checksum "4aa8223…"^^spdx:sha256Hex.
     
The moves the digest algorithm into the datatype specification of the
literals.  This approach seems quite elegant.  There is a single
property so it is easy for tools deal with.  It is extensible, anyone
could define a new datatype.  It is relatively compact.

However, there very few ontologies that use xml datatypes in this way.
This could be because there are subtle problems with this approach.
Or it could be that it is just uncommon.  This approach would break
down for algorithms that have any secondary parameters.  In that case
you could combine it with the third option, though.

A graph for the third option would look like

    <http://zlib.net/zlib-1.2.5.tar.gz#Makefile> a spdx:file;
      spdx:checksum [spdx:algorithm <spdx:sha1>;
                     spdx:checksumValue "1fac389…"^^xs:hexBinary];
      spdx:checksum [spdx:algorithm <spdx:sha256>;
                     spdx:checksumValue "4aa8223…"^^xs:hexBinary].
      
In many ways this approach is very similar to the datatype based one.
The introduction of anonymous resources does, potentially, allow the
addition of additional parameters to the digest algorithm.  However,
tools that do not understand the algorithm would probably not pass
that information though correctly.  One practical upside is that the
label for particular algorithms can be stored in the RDF.  The
`<spdx:sha1>` resource can have a `dc:title` property with value
"SHA1".  This would mean that tools don't necessarily have to
implicitly understand a digest type in order to display it to humans.

In the end we decided to use the third approach on SPDX.  The
additional flexibility was generally found appealing.  Having to
define a new property for each new digest algorithm was generally
viewed as a bit of a kludge.  When i presented this issue to the
semantic web mailing list there was only one response which preferred
the first option, but found the third option acceptable.  Most of the
people involved in the SPDX effort are not highly experienced RDF
modelers.  I am not sure if the distaste for defining new properties
reflects our relative lack of experience with RDF or if it is more
fundamental.

Feedback on this choice is welcome but this post is more of an
exploration of the possibilities and implications of those approaches.

    

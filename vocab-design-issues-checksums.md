When designing [RDF][] vocabularies one often encounters scenarios
where a choice must be made between multiple viable approaches of
modeling the information.  The [SPDX][] technical team recently
encountered such a situation.

[RDF]: http://www.w3.org/RDF/
[SPDX]: http://spdx.org/

The exact scenario was: the information we store about a file is
potentially invalidated with every change to the contents of that
file.  Some code might be added that has different licensing
requirements, for example. To make sure that the information in and
SPDX file is valid for a particular file it must be possible to verify
that its contents are identical to the contents of the file that was
analyzed.

In SPDX we decided to accomplish this with the use of hash functions.
Every file that is in an SPDX file should have at least one hash
function based checksum associated with it.  However, hash function
come and go.  SHA1 is current very popular but is steadily being
replaced with with SHA256 and SHA512.  We wanted to be able to support
more than one hash algorithm.

We had three basic choices: 

 * have separate properties for each checksum algorithm
 * define datatypes for each checksum algorithm and have single
   `checksum` property
 * define a class for checksums that encapsulates all the data and
   have a single `checksums` property
   
For the first option the graph would look like

    <http://zlib.net/zlib-1.2.5.tar.gz#Makefile> a spdx:file;
      spdx:sha1 "1fac389..."^^xs:hexBinary;
      spdx:sha256 "4aa8223..."^^xs:hexBinary.
      
That is pretty simple and self explanatory.  Adding new checksum
algorithms requires the addition of new properties.  One potential
downside is that some tools might not be able to pass through
checksums for algorithms the do not understand.
 
For example, SPDX provide a tool to translate SPDX RDF data to and
from spreadsheets.  The checksums inserted into the spreadsheet by
looking for the known checksum properties and putting that data into
the appropriate column in the spreadsheet.  This means that checksums
of novel types would be lost in the translation.  This could be
avoided if the tool supported [OWL][] inferencing but it is unlikely
we will get to that in the near future.  I think requiring OWL
inferencing to work properly is a design smell.
     
[owl]: http://www.w3.org/TR/owl-ref/

A graph for the second option would look like

    <http://zlib.net/zlib-1.2.5.tar.gz#Makefile> a spdx:file;
      spdx:checksum "1fac389..."^^spdx:sha1Hex;
      spdx:checksum "4aa8223..."^^spdx:sha256Hex.
     
The moves the checksum algorithm into the datatype specification of
the literals.  This approach seems really elegant to me.  There is a
single property so it is easy for tools deal with.  It is extensible,
anyone could define a new datatype.  It is relatively compact.

However, very few people seem to use xml datatypes in this way.  This
could be because there are subtle problems with this approach.  Or it
could be that it is just uncommon.  This approach would break down for
algorithms that have any parameters.  In that case you could combine
it with the third option, though.

A graph for the third option would look like

    <http://zlib.net/zlib-1.2.5.tar.gz#Makefile> a spdx:file;
      spdx:checksum [spdx:algorithm <spdx:sha1>;
                     spdx:checksumValue "1fac389..."^^xs:hexBinary].
      spdx:checksum [spdx:algorithm <spdx:sha256>;
                     spdx:checksumValue "4aa82e..."^^xs:hexBinary].
      
In many ways this approach is very similar to the datatype based one.
The introduction of anonymous resources does, potentially, allow the
addition of additional parameters to the checksum algorithm.  However,
tools that do not understand the algorithm would probably not pass
that information though correctly.  One real upside is that the label
for particular algorithms can be stored in the RDF.  The `<spdx:sha1>`
resource can easily have a `dc:title` that tools can use when
displaying that information to humans.

In the end we decided to use the third approach on SPDX.  The
additional flexibility was generally found appealing.  Specifically,
having to define a new property for each new checksum algorithm was
generally viewed as a bit of a kludge.  (It is worth noting that most
of us involved in the SPDX effort are not experienced RDF modelers.)
When i presented this issue to the semantic web mailing list there was
only one response which preferred the first option, but found the
third option acceptable.  I am not sure if the distaste for defining
new properties reflects our relative lack of experience with RDF or if
it is more fundamental.

    

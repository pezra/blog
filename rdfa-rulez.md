The tension between human and machine readability is never greater
than when developing interchange formats.  Formats that are easy and
efficient for computers to read tend to be rather difficult for people
to understand.  When developing an interchange format you know that
there will be few tools supporting it when it is released tools so it
needs to be useful even with limited tooling.  However, the format
must support the development of sophisticated tools if it is to
succeed in the long run.

A large part of the appeal of XML based languages is XML provides a
reasonable balance between those two factors -- for programmers.  It
can be read easily by computers and understood reasonably well by a
programmer with very limited tooling.

For the non-programmer the story is somewhat different.  Most XML
based languages are complete gibberish to people without significant
technical expertise.  A business person will need non-trivial tools to
allow them to consume the information locked up in an XML file.

Data interchange formats implicitly value the wider distribution of
information.  Why else would you be exchanging data.  It is
disappointing that so many of these formats are based on technology
the excludes all but those with sophisticated tools or deep technical
knowledge.  Data interchange formats should be designed first for
people, and second for computers.  A properly designed data
interchange format should be consumable, using commonly available
tools, by any person who is familiar with the domain.

This means that XML is pretty much right out.

Fortunately there is [HTML+RDFa][rdfa].  RDFa allows [RDF][] data sets
to be serialized into HTML documents.  The information can then be
consumed by humans using any web browser.  The raw data can be readily
extracted by tools.

Consider the following two examples.  Each is part of an SPDX[^spdx]
file.  In first example, HTML+RDFa, both groups are easily supported.
The information is displayed in a way that it can be understood by
most human and the data is machine readable.  In the second the
information is machine readable but quite difficult for humans to
interpret.

[^spdx]: The [Software Package Data Exchange][spdx] project is
designing a way to exchange licensing information for software
packages.  The current phase of development is primarily focused on
simple manifest and copyright licensing related information.

<div style="border: solid 2px gray;">
<h2>Files in zlib 1.2.5</h2>
<table>
<thead>
<tr><th>Name</th><th>Type</th><th>License</th><th>Checksum</th><th>Copyright</th></tr>
</thead>

<tbody>
<tr typeof="spdx:File" about="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&amp;package_version_id=3690#adler32.c">
<td property="spdx:Name">adler32.c</td>
<td property="spdx:Type">source</td>
<td><a href="http://spdx.com/licenses/zlib" rel="spdx:License">Zlib</a></td>
<td property="spdx:mac" datatype="spdx:sha256">gWAPnq8fV6sVKdiYkgJQ1nFoTaXXSqoVfJbMCr9Kzd0</td>
<td property="spdx:Copyright">unknown</td>
<span resource="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&amp;package_version_id=3690" rev="spdx:MemberFile"/>
</tr>

<tr typeof="spdx:File" about="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&amp;package_version_id=3690#amiga/Makefile.pup">
<td property="spdx:Name">amiga/Makefile.pup</td>
<td property="spdx:Type">other</td>
<td><a href="http://spdx.org/licenses/Zlib" rel="spdx:License">Zlib</a></td>
<td property="spdx:mac" datatype="spdx:sha256">plyzzUCxuOx34oiXTdncU9ke14u.SV6UzMhN3UI.3x8</td>
<td property="spdx:Copyright">unknown</td>
<span resource="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&amp;package_version_id=3690" rev="spdx:MemberFile"/>
</tr>
        
        
<tr typeof="spdx:File" about="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&amp;package_version_id=3690#ChangeLog">
<td property="spdx:Name">ChangeLog</td>
<td property="spdx:Type">other</td>
<td><a href="http://spdx.org/licenses/Zlib" rel="spdx:License">Zlib</a></td>
<td property="spdx:mac" datatype="spdx:sha256">rxKcRCSHu8.4tHMdiJRINMatY4efBCMz.PmHpo.gj9s</td>
<td property="spdx:Copyright">unknown</td>
<span resource="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&amp;package_version_id=3690" rev="spdx:MemberFile"/>
</tr>

<tr typeof="spdx:File" about="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&amp;package_version_id=3690#contrib/ada/readme.txt">
<td property="spdx:Name">contrib/ada/readme.txt</td>
<td property="spdx:Type">other</td>
<td><a href="http://spdx.org/licenses/GPL-2.0" rel="spdx:License">GPL-2.0</a></td>
<td property="spdx:mac" datatype="spdx:sha256">j.nlMD8ujot0bHglDnS3xK63zmIS_c51H8Ogzlakf.I</td>
<td property="spdx:Copyright">unknown</td>
<span resource="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip&amp;package_version_id=3690" rev="spdx:MemberFile"/>
</tr>
</tbody>
</table>
</div>

The following is a similar amount of information expressed in RDF/XML

    <spdx:File rdf:about="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip=3690#CMakeLists.txt">
      <spdx:Copyright>unknown</spdx:Copyright>
      <spdx:License rdf:resource="http://spdx.org/licenses/Zlib"/>
      <spdx:Name>CMakeLists.txt</spdx:Name>
      <spdx:Type>source</spdx:Type>
      <spdx:mac rdf:datatype="http://purl.org/spdx/sha256">L_NaoTEuHjO8uI1VLGIai2rDk7wPkoyeSe5vVD1gxOc</spdx:mac>
    </spdx:File>
    <spdx:File rdf:about="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip=3690#ChangeLog">
      <spdx:Copyright>unknown</spdx:Copyright>
      <spdx:License rdf:resource="http://spdx.org/licenses/Zlib"/>
      <spdx:Name>ChangeLog</spdx:Name>
      <spdx:Type>other</spdx:Type>
      <spdx:mac rdf:datatype="http://purl.org/spdx/sha256">rxKcRCSHu8.4tHMdiJRINMatY4efBCMz.PmHpo.gj9s</spdx:mac>
    </spdx:File>
    <spdx:File rdf:about="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip=3690#FAQ">
      <spdx:Copyright>unknown</spdx:Copyright>
      <spdx:License rdf:resource="http://spdx.org/licenses/Zlib"/>
      <spdx:Name>FAQ</spdx:Name>
      <spdx:Type>other</spdx:Type>
      <spdx:mac rdf:datatype="http://purl.org/spdx/sha256">qNaKL48VknhfAA7NgpOLjMoyHlxv45x.hMu8UfAy0Fc</spdx:mac>
    </spdx:File>
    <spdx:File rdf:about="https://olex.openlogic.com/package_versions/download/9423?path=openlogic/zlib/1.2.5/openlogic-zlib-1.2.5-all-src-1.zip=3690#INDEX">
      <spdx:Copyright>unknown</spdx:Copyright>
      <spdx:License rdf:resource="http://spdx.org/licenses/Zlib"/>
      <spdx:Name>INDEX</spdx:Name>
      <spdx:Type>other</spdx:Type>
      <spdx:mac rdf:datatype="http://purl.org/spdx/sha256">HMjPi3ZRY2Anq1vc4Lfl5x77JWj3hhaWWT.I34QPK9g</spdx:mac>
    </spdx:File>


The extreme accessibility of HTML+RDFa for both humans and machines
makes it an obviously superior choice for data interchange formats.
HTML+RDFa is a relatively new entry into the arena.  Hopefully we will
see more data formats use this superb technology.
 

[spdx]:http://spdx.org/
[rdf]: http://www.w3.org/TR/rdf-primer/
[rdfa]: http://www.w3.org/TR/xhtml-rdfa-primer/


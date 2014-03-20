[HalClient][] is yet another ruby client for [HAL][] based APIs. The
goal is to provide an easy to use set of abstractions on top of HAL
without completely hiding the HAL based API underneath. The areas of
complication that HalClient seeks to simplify are

 * [CURIE][] links
 * regular vs embedded links
 * templated links
 * working [RFC6573][] collection

Unlike many other ruby HAL libraries HalClient does not attempt to
abstract HAL away in favor of domain objects. Of course, we want
domain objects but HalClient leaves that to the application code.

CURIEs
---

CURIEd links are often misunderstood by new users of HAL. Dealing with
them is not hard but it requires care to do correctly. Failure to
implement CURIE support correctly will result in future breakage as
services make minor syntactic changes to how they encode
links. HalClient's approach is to treat CURIEs a purely over-the-wire
encoding choice. Looking up links in HalClient is always done using
the full link relation.


[comverge]: http://comverge.com
[hal]: http://stateless.co/hal_specification.html
[halclient]: http://github.com/pezra/hal-client
[CURIE]: 
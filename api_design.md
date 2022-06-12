# Node on API design

### Postel's law

https://en.wikipedia.org/wiki/Robustness_principle 

> Be conservative in what you send, be liberal in what you accept

Similar to a rule to design compatible function

> Be contravariant in the input type and covariant in the output type

In a good sense, the rule suggests you to accept as many "good" inputs as possible, but could be viewed as "try to accept 'anything'", 
which translated as a security issue (which is criticized by Martin Thomson, and later quoted in a 
[paper]([url](https://petsymposium.org/2018/files/papers/issue2/popets-2018-0011.pdf)) to exploit TOR protocol)


### Open API 3.0

https://swagger.io/specification/ 

- Open API spec has more detail than JSON schema (eg: support 'format' beside 'type' to distinguish between 'int32', 'int64').
- Each API has 'Info' object to specify, title, description, and even contact to find owner of the API


### Avoid actions, think about resources

REST should be modelled around 'Resource' (why RPC is modelled around 'Action').
From this aspect, we may notice that REST is pretty bad by model everything by string, split by path (while RPC has 'service' and 'message', 
GraphQL separate 'mutation' and 'data')

Also, sub resources should be seperated by path segment (`/resources/:id/sub-resources/:another-id`)

### Conventional query parameters

|        |                                                             |
|--------|-------------------------------------------------------------|
| q      | query parameter                                             |
| sort   | list of fields to define sort order (+ for asc, - for desc) |
| fields | retrieve subset of fields                                   |
| offset | pagination                                                  |
| cursor | pagination                                                  |
| limit  | pagination                                                  |


### PATCH vs PUT

PUT is to update the 'whole' resource, while PATCH is to update parts of the resource. In that sense, PUT should be renamed to 'REPLACE' intead?
There are few standards come with PATCH

- [JSON Merge Patch]([url](https://datatracker.ietf.org/doc/html/rfc7396))
- [JSON Patch]([url](https://datatracker.ietf.org/doc/html/rfc6902))


### If-Match / If-None-Match header

Useful to provide idempotency. Also, could design along with Etag to return a hash of cached response (if match).
See also https://datatracker.ietf.org/doc/html/rfc7232 

### Prefer header

`Prefer` head is useful when clients want to provide how they expect the server to process the data: eg: respond-async, wait time...


### Richardson maturity model

https://martinfowler.com/articles/richardsonMaturityModel.html#level2 








Also see http://erosb.github.io/post/json-patch-vs-merge-patch/ 



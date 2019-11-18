# OGC Web API Guidelines

## A Comprehensive Set of Guidelines for developing OGC Web APIs 

The OGC Web API Guidelines consist of a list of Design Principles.

## Why OGC Web API Design Principles 

The following is a set of common design principals for developing Web APIs.

- The design of a common elements of OGC Web APIs should follow a common pattern to ensure easy adoption.

- There are common design principles in main-stream IT that should be adopted to ease the adoption of OGC Web APIs.

- But still, there are some aspects that would need to be agreed upon to ensure seamless APIs for different thematic topics in OGC.

- In particular avoid that APIs are fundamentally different to access, process and manage different kinds of geospatial resources such as Features, Maps, Tiles, Coverages, Observations, Processes, etc.

## Purpose and Process

The implementation of Web APIs that allow the management, processing and use of geospatial information should be possible by anyone familiar with the Web APIs designed for main stream IT. However, when designing a Web API by multiple domain experts, and not only by one team, and when trying to address multi purpose usage, it becomes challenging to ensure a common design pattern among all teams. 

To ensure that (i) Web API design across all different domains of expertise is coherent and (ii) the maximum from main stream IT design pattern is reused, the OGC Architecture Board (OAB) requested the elaboration of these guidelines. This process is done under the Architechture DWG with the colaboration of the OWS Common SWG. For the moment this is a living document to inspire additional discussion and refinement within and among DWG and SWG teams, and contribute our learnings and suggestions to the technological community at large. The final aim is to reach consensus and converge in a document that the OGC can approve as a Policy Document. 

Even though functioning as a Policy Document, the use is more like a checklist to streamline the design and the review process in OAB and OGC Technical committee. The assessment from verifying the Web API design against the checklist should be submitted with the Web API draft standard to the OAB. It is possible to not follow all principles, but reasons for deviation should be given.

For the moment you should consider this to be a living, evolving document. Please create or comment on existing Issues to discuss changes, corrections, and enhancements to the principles. 

## Starting point

The starting point of the Design Principles listed here was taken from a presentation [OGC Web API Design Principles](https://portal.opengeospatial.org/files/?artifact_id=78344) (requires OGC portal login) given during the OGC TC meetings in Orleans and Fort Collins. The presentation summerizes a collection of the Web API design principles used today by major players in main stream IT business. The purpose of the presentation is to ensure that the "common part of an API" is designed such that it can be re-used and a adopted easily. However, the initial presentation was not perfect in the sense that it might be incomplete and that there is room left for a good consensus discussion.

The original author of the presentation (Andreas Matheus), in collaboration with Charles Heazel, agreed to make the content available in this open GitHub repo for the purpose of creating a starting point in discussion and deriving a set of guidelines that could eventually be used to test OGC Web API Implementation Standards for conformance.

## Design Principles

### Principle #1 – Don’t Reinvent

For aspects and functional capabilities that are already solved in main-stream IT and meet geospatial community requirements, simply adopt these API elements.

Focus instead on geo-centric and domain specific requirements to create new APIs or extend existing APIs. 

### Principle #2 – Keep It Simple and Intuitive

Make the developer of the API successful as quickly as possible!

### Principle #3 - Use Well-Known Resource Types

Identify your resource types and reuse existing definitions from the OGC Naming Authority resource type register (to be established). 

Encodings of resource types should be associated with an IANA registered media type.

### Principle #4 – Construct consistent URIs

Great Web APIs look like they were designed by a single team. The most obvious properties of an API are the access paths and the URL templates which define them. Therefore, OGC conventions for the construction of access path templates are essential. Some of these templates are emerging though the Web Feature Service 3.0 efforts. Before creating a new URI scheme, you should follow and build on existing approaches in OGC. If creating your own URI scheme, please explain your URI pattern.

Your API URI pattern should be documented, formalized, explained.

One existing approach in OGC is the following (simplified):

For resource types that consist of a collection of resources, the pattern at the end of the URI path is as follows where `resourceType` is in plural:

    .../{resourceType}/{resourceId}

Where resources are nested, the path elements may be concatenated. For example: 

    .../collections - returns the list of feature collections
    .../collections/highways - returns representation of the collection 'highways'
    .../collections/highways/items - returns the features in the collection 'highways'
    .../collections/highways/items/A8 - returns the feature 'A9' in the collection 'highways'
    
For resource types that consist of a single resource, the pattern at the end of the URI path is as follows where `resourceType` is in singular:

    .../{resourceType}

For example:

    .../collections/highways/schema - returns the schema for the features in the collection 'highways'
    .../collections/highways/metadata - returns the information about the features in the collection 'highways'

Note that it doesn’t matter if you use singular or plural for your nouns to build the paths, but use a consistent pattern throughout your API!

### Principle #5 – Use HTTP Methods consistent with RFC 2616

Include in your API design the use of all HTTP methods that operate on resources: **GET, POST, PUT, DELETE**

Define the semantics carefully when a method is invoked on a particular URI addressing a resource. E.g.

| Resource  | POST  | GET  | PUT | DELETE 
| -- | -- | -- | -- | -- 
| ../collections/highways/items | create a new highway | list all highways | bulk update of highways | delete all highways  
| .../collections/highways/items/A8 | Error! | show A8 | If exists: Update A8 else: Create A8 | delete highway A8

Do not force all semantics in just HTTP GET!

Also consider support for other HTTP methods:

 * HEAD to return HTTP Headers with no payload
 * OPTIONS to support W3C CORS
 * PATCH to update parts of an existing resource

### Principle #6 – Put Selection Criteria behind the ‘?’

The radical idea behind the '?' concept is that everything **left** of the **'?'** (the path design) identifies a resource and that everything **right** of the **'?'** may select specific representaion(s) of parts or the entire resource.

For example:

    .../collections/highways/items?id=A8 => returns highway A8
    .../collections/highways/items?id=A8,A9 => returns highways A8 and A9

    .../collections/highways/items/A8?time=2019-02-12T12:00:00Z => returns a highway A8 represetation at the given time

If you define a query parameter on a resource, define the API behaviour in all cases including error situations. For example, if you 
support an `id` parameter on a feature resource then define the semantics of the following query examples:

    .../collections/highways/items/A8?id=A8 => should the request return *true* or *the resource itself*?
    .../collections/highways/items/A8?id=A81 => should the request return *false* or '*NULL*' (assuming the id of A8 is not A81)?

Another example for a query parameter could be `properties` (which can also be combined with other parameters):

    .../collections/highways/items/A8?properties=name,geometry => return the highway A8, but only the name and geometry attributes
    .../collections/highways/items/A8?time=2019-02-12T12:00:00Z&properties=name,geometry => return the highway A8 at the given time, but only the name and geometry attributes

Use of the query string to select resources is highly resource specific and must be described on a case by case basis.

### Principle #7 – Error Handling and use of HTTP Status Codes

*Note: Error Codes are the developers insight into your API. So be precise and as detailed as possible. Error handling is often one of the biggest complaints when using an API.*

Associate each error situation of your API with the appropriate HTTP status code (see also Principle #8).

However, you may also consider supporting a "switch off" that always returns a status code 200 plus additional (debug / insight) information in the HTTP response body

    e.g. ?suppress_response_codes=true

Return detailed human readable error no. + description +
information on how to fix things + contact details

    { "developer_message": "…",
      "user_message": "...",
      "error_code": "...",
      "contact_details": "..."  
    }

### Principle #8 – Use of HTTP Status Codes

More then 70 HTTP status codes exist (summary in RFC 7231).  You should reduce the use in your API to the most important ones. For example:

| Option Set #1 – Basic set |  Option Set #2 – Additional
| -- | -- 
| - 100 - Continue |
| - 200 - OK | - 201 - Created 
|  | - 204 - No Content
|  | - 304 - Not Modified 
| - 400 - Bad Request | - 401 Unauthorized  
|  | - 403 - Forbidden 
|  | - 404 - Not Found 
|  | - 405 - Method Not Allowed 
|  | - 406 - Not Acceptable
|  | - 409 - Conflict 
|  | - 410 - Gone 
|  | - 412 - Precondition Failed 
|  | - 415 - Unsupported Media Type
|  | - 422 - Unprocessable Entity
|  | - 429 - Too Many Requests
| - 500 - Internal Server Error | - 503 - Service Unavailable 

Be explicit which 30x statucs code your API supports. For any supported 30x follow the HTTP semantics.

### Principle #9 – Use of HTTP Header

Define all HTTP Headers that your API supports.

Use HTTP Headers as intended by RFC 2616, but design your API to allow overwriting of HTTP Headers based on URL query parameters.

For support of caching consider to support entity tags and the associated headers. However, their use might be in conflict when implementing security requirements. For these cases, you should explicitly name those headers that must be overwritten to avoid caching.

### Principle #10 - Content Negotiation

Content negotiation is an important, but special case of Principle #9. 

Use HTTP request header like 'Accept' or 'Accept-Language' to request the response in a particular content type or language as defined in [RFC 2616](https://tools.ietf.org/html/rfc2616).

Use registered [IANA Media Types](https://www.iana.org/assignments/media-types/media-types.xhtml) whenever possible. 

An example for content negotiation based on HTTP headers and with query parameter override:

   HTTP 1.1 GET .../collections/highways/items/A8
   accept: application/geo+json
   => should return the response using the GeoJSON encoding

   HTTP 1.1 GET .../collections/highways/items/A8?accept=application%2Fgml%2Bxml
   accept: application/json
   => should return the response using the GML encoding


### Principle #11 - Pagination

APIs for large data collections should support pagination.

For example, use **limit** and **offset** as "query-string" parameters.

    .../collections/highways/items?limit=50&offset=101 => returns upto 50 highways starting at position 101

The API should return metadata with each response providing the total number of resources available (e.g. total) in the payload as well as the link to the next page.

As a supplement consider support for Web Linking (RFC 5988)

– Use HTTP Response Header to provide URLs for fetching the next / previous page
– This approach is application neutral and should be provided by the API as the default

### Principle #12 – Processing Resources

Use **verbs** to offer **operations** on resources, for example:

    .../transform => represents a processing resource that allows to transform another resource

The parameters of the process are provided as query parameters, for example:

    .../transform?in=.../collections/highways/items/A8&toCRS=http://www.opengis.net/def/crs/EPSG/0/3258 => returns the A8 highway in the coordinate reference system ETRS89 lat/long

Note that the result of the example above may result in the same response as a selection/negotiation parameter on the resource (see Principle #6), for example:

    .../collections/highways/items/A8?crs=http://www.opengis.net/def/crs/EPSG/0/3258

APIs may decide to offer processing resources as separate operations to support an explicit separation and highlight the processing capability. This allows to publish explicit metadata about the process, e.g., the input and output data structures.

### Principle #13 – Support Metadata

This part of the API helps the developer to understand how to use data or processing resources. Two approaches exist how to achieve this:

(1) Start the URL path with 'metadata' to indicate that subsequent path identifies a resource for which the metadata is returned.

    .../metadata/collections/highways/items/A8

(2) End the URL path with 'metadata' to indicate that the metadata is an integral part of the resource that can be fetched sepreately.

    .../collections/highways/items/A8/metadata

Regardless of the approach taken, use it consistently.
    
You may use of the ‘?’ operator to send selection criteria (see Principle #6).

### Principle #14 – Consider your Security needs

Try to follow common practices for security in Web APIs, for example:

- Host your API on HTTPS.
- Include support for authentication for the beginning. 
- Consider consistent support for CRUD (Create, Read, Update, Delete) from the beginning (see Principle #5);
- support for Execute may be provided on processing resources (see Principle #12) or using POST (see Principle #5).

### Principle #15 – API Description

Describing the API in human and macine readable form has value to the developer. Currently OpenAPI version 3 is common practice.

### Principle #16 - Use IANA well-known identifiers

IANA and other standardization organizations have defined so called well known identifiers for different purposes. For example:

- Media types: https://www.iana.org/assignments/media-types/media-types.xhtml
- Link relations: https://www.iana.org/assignments/link-relations/link-relations.xhtml
- Well-known URIs: https://www.iana.org/assignments/well-known-uris/well-known-uris.xhtml

For example is it possible to differenciate between XACML or GeoXACML policies. XACML policies would be returned with the 'application/xacml+xml' media type and GeoXACML policies with media type 'application/geoxacml+xml'

### Principle #17 - Use explicit geospatial relations

In many cases it is appropiate to use typed relation to explicitelly declare links among resources. A special case are topological spatial relations between resources (e.g., contains, within, etc.) which are easy to derive with a GIS, but not with Web clients unless the relations are explicitly represented. The relations may either be explicitly included in the resource representation or in Link headers in the HTTP response header (see RFC 5988).

### Principle #18 - Support W3C Cross-Origin Resource Sharing

If your Web API is accessed by Web-applications executed in a Web Browser, support W3C CORS (https://www.w3.org/TR/cors/). This allows to overcome the security restrictions introduced by the Same-Origin Policy (https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) applied by the Web Browser to JavaScript based applications when trying to access your Web API.

In cross origin cases, as identified in W3C CORS, the HTTP request carries specific HTTP headers and it is expected by the Web Browser that associated HTTP response headers exist in the response. Otherwise the processing stops.

### Principle #19 - Resource encodings

The API should provide resource representations based on the expectations of the developers.

You also have to decide whether or not the Web API should support a default encoding that every implementation has to support. You should recommend to support JSON and HTML as encodings for all resources. JSON is recommended as it is  a commonly used format that is simple to understand and well supported by tools and software libraries; HTML is recommended as it is the standard encoding for Web content.

Still, the XML encoding should be supported as it is often required to meet specific security requirements. Also, many existing standards and OGC encodings are based on XML.

### Principle #20 - Good APIs are testable from the beginning

Any OGC Web API developed according to these guidelines can be tested at design phase already. Considering all design principles including the identification of resource types, the effect of applying HTTP methods to them, the potential HTTP status codes, etc. provides the basis for documenting and implementing compliance tests in parallel to the API design.

### Principle #21 - Specify wether operations are safe and/or idempotent
For each operation one has to specify whether it has to be *safe* and/or *idempotent*. This is important, because clients and middelware rely on this.

> **Safe (read-only)**
>
> Safe (read-only) in this case means that the semantics has been defined as read-only. This is important, because clients and middelware like to use caching.

> **Idempotent**
>
> Idempotent means that multiple, identical requests have the same effect as one request.

|Operation|Safe|Idempotent|
|-|-|-|
|`POST`|No|No|
|`GET`, `OPTIONS`, `HEAD`|Yes|Yes|
|`PUT`|No|Yes|
|`PATCH`|No|Optional|
|`DELETE`|No|Yes|
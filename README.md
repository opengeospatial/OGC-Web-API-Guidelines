# OGC Web API Guidelines

## A Comprehensive Set of Guidelines for developing OGC Web APIs 

The OGC Web API Guidelines consist of a list of Design Principles inspired by mainstream IT. Examples include:

- [ZalandZalando RESTful API and Event Scheme Guidelines](https://opensource.zalando.com/restful-api-guidelines/)
- [Microsoft REST API Guidelines](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md)
- [Google API Design Guide](https://cloud.google.com/apis/design/)

## Why OGC Web API Design Principles 

This document presents a set of common design principals for developing OGC Web APIs Implementation Standards.

- The design of common elements of OGC Web APIs should follow a common pattern to ensure consistency, enhanced interoperability and easier adoption.

- There are common design principles in main-stream IT that should be adopted to ease the development, adoption and use of OGC Web APIs.

- There are aspects related to OGC Web API specification that need to be agreed upon to ensure consistent APIs for different geospatial technology, information domains, and thematic topics currently used in the OGC.

- In particular the goal is to avoid that OGC Web APIs are fundamentally different to accessing, processing and managing different kinds of geospatial resources such as Features, Maps, Tiles, Coverages, Observations, Processes, etc.

## Purpose and Process

The implementation of Web APIs that allow the management, processing and use of geospatial information should be possible by anyone familiar with Web APIs designed for main stream IT. However, when an OGC Web API is being designed by multiple domain experts, while trying to address multipurpose usage, ensuring that a common design pattern is used among and between all SWGs can be challenging. 

To ensure that (i) OGC Web API design and specification across all different domains of expertise is coherent and (ii) the maximum main stream IT design patterns are reused, the OGC Architecture Board (OAB) requested elaboration and documentation of OGC Web API development guidelines. This process was performed in the Architecture DWG with the collaboration of the OWS Common SWG. At that time, this was a living document formulated to inspire additional discussion and refinement within and between OGC Working Groups and to contribute our learnings and suggestions to the broader technology community that depends on geospatial resources. The final aim is to reach consensus and converge in a document that the OGC Members can approve as a Policy Document. 

All principles are equally important and the order of the principles does not reflect their relative importance.

Even though the main goal is to provide OGC Web API development guidance, a key secondary objective is to have a checklist to streamline the OAB and Technical Committee review process. The assessment for verifying the Web API design against the checklist should be submitted with the Web API draft standard to the OAB. While following all principles is not mandatory, reasons for deviation should be given.

For the moment please consider version 1.x to be the foundation for further discussion and consensus. Please create or comment on existing Issues to discuss changes, corrections, and enhancements to the principles for creating revisions.

## Starting point

The starting point for developing the Design Principles listed in this document was taken from a presentation [OGC Web API Design Principles](https://portal.opengeospatial.org/files/?artifact_id=78344) (requires OGC portal login) given during the OGC TC meetings in Orleans, France and Fort Collins, Colorado, USA. The presentation summarized a collection of the Web API design principles used today by major players in main stream IT business. The purpose of the presentation was to ensure that the "common part of an API" is designed such that it can be re-used and a adopted easily. However, the initial presentation was incomplete and  there is room for a good consensus discussion.

The original author of the presentation (Andreas Matheus), in collaboration with Charles Heazel, agreed to make the content available in this open GitHub repo for the purpose of creating a starting point in discussion and deriving a set of guidelines that will be used by the OGC Architecture Board to test OGC Web API Implementation Standards for consistency before approving RFC.

## Design Principles

### Principle #1 – Don’t Reinvent

For aspects and functional capabilities that are already solved in main-stream IT and meet geospatial community requirements, simply adopt these API elements.

Focus instead on geo-centric and domain specific requirements to create new APIs or extend existing APIs. 

### Principle #2 – Keep the API Simple and Intuitive

Make the implementer of the API successful as quickly as possible.

Avoid astonishment by defining a (default) behavior of the Web API that is predicable by the users (see principle #23).

### Principle #3 - Use Well-Known Resource Types

Identify your resource types and reuse existing definitions from the OGC Naming Authority resource type register (to be established). 

Encodings of resource types should be associated with an IANA registered media type. For more guidance see Principle #16.

### Principle #4 – Construct consistent URIs

Great Web APIs look like they were designed by a single team. The most obvious properties of an API are the access paths and the URL templates which define them. Therefore, OGC conventions for the construction of access path templates are essential. Before creating a new URI scheme, the SWG should follow and build on existing OGC approaches that have proven to work. If the SWG is creating a URI scheme, please explain the URI pattern.

The API URI pattern should be documented, formalized, explained.

One existing approach in the OGC is the following (simplified):

For resource types that consist of a collection of resources, the pattern at the end of the URI path is as follows where `resourceType` is plural:

    .../{resourceType}/{resourceId}

Where resources are nested, the path elements may be concatenated. For example: 

    .../collections - returns the list of feature collections
    .../collections/highways - returns representation of the collection 'highways'
    .../collections/highways/items - returns the features in the collection 'highways'
    .../collections/highways/items/A8 - returns the feature 'A8' in the collection 'highways'
    
For resource types that consist of a single resource, the pattern at the end of the URI path is as follows where `resourceType` is singular:

    .../{resourceType}

For example:

    .../collections/highways/schema - returns the schema for the features in the collection 'highways'
    .../collections/highways/metadata - returns the information about the features in the collection 'highways'

Note that it does not matter if singular or plural is used for nouns to build the paths, but use a consistent pattern throughout the API!

### Principle #5 – Use HTTP Methods consistent with RFC 7231

Include in the API design the use of all HTTP methods that operate on resources: **POST, GET, PUT, ,PATCH, DELETE**

The most important design aspect of REST is the seperation of the API in logical resources (*things*). The resources describe the information of the *thing*. These resources are manipulated using HTTP-requests and HTTP-operations. Each operation (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`) has a specific meaning.
> HTTP also defines operations, e.g. `TRACE` and `CONNECT`. In the context of REST, these operations are hardly ever used and have been excluded from the rest of the overview below.

|Operation|CRUD|Description|
|-|-|-|
|`POST`|Create|Create resources (i.e. `POST` adds a resource to a collection).|
|`GET`|Read|Retrieve a resource from the server. Data is only retrieved and not modified.|
|`PUT`|Update|Replace a specific resource. Is also used as a *create* " if the resource at the indicated identifier/URI does not exist yet.|
|`PATCH`|Update|Partially modify an existing resource. The request contains the data and instructions that have to be used for changing/modifying the resource. In case JSON is the encoding format in the designated JSON merge patch format (RFC 7386).|
|`DELETE`|Delete|Remove the specific resource.|

Also consider support for other HTTP methods:

 * HEAD to return HTTP Headers with no payload
 * OPTIONS to support W3C CORS and Web API specific semantics

Allow the use of `POST` as an alternative for a complext `GET` operation.

### Principle #6 – Put Selection Criteria behind the ‘?’

The rational idea behind the '?' concept is that everything **left** of the **'?'** (the path design) identifies a resource and that everything **right** of the **'?'** may select specific representaion(s) of parts or the entire resource.

For example:

    .../collections/highways/items?id=A8 => returns highway A8
    .../collections/highways/items?id=A8,A9 => returns highways A8 and A9

    .../collections/highways/items/A8?time=2019-02-12T12:00:00Z => returns a highway A8 represetation at the given time

If a query parameter is defined on a resource, define the API behaviour in all cases including error situations. For example, if an `id` parameter on a feature resource is supported then define the semantics of the following query examples:

    .../collections/highways/items/A8?id=A8 => should the request return *true* or *the resource itself*?
    .../collections/highways/items/A8?id=A81 => should the request return *false* or '*NULL*' (assuming the id of A8 is not A81)?

Another example for a query parameter could be `properties` (which can also be combined with other parameters):

    .../collections/highways/items/A8?properties=name,geometry => return the highway A8, but only the name and geometry attributes
    .../collections/highways/items/A8?time=2019-02-12T12:00:00Z&properties=name,geometry => return the highway A8 at the given time, but only the name and geometry attributes

Use of the query string to select resources is highly resource specific and must be described on a case by case basis.

### Principle #7 – Error Handling and use of HTTP Status Codes

*Note: Error Codes are the implementer's insight into the API and must be precise and detailed as possible. Inconsistent or inaccurate or ambiguous error handling is often one of the biggest complaints when using a Web API*

Associate each error situation of the API with the appropriate HTTP status code (see also Principle #8).

However, you may also consider supporting a "switch off" that always returns a status code 200 plus additional (debug / insight) information in the HTTP response body

    e.g. ?suppress_response_codes=true

Return detailed human readable error no. + description +
information on how to fix things + contact details

    { "developer_message": "…",
      "user_message": "...",
      "error_code": "...",
      "contact_details": "..."  
    }

### Principle #8 – Use explicit list of HTTP Status Codes

More than 70 HTTP status codes exist (summary is in RFC 7231).  You should reduce the use in your API to the most important ones. Use a concise list in your API documentation (see principle API description), for example:

| Option Set #1 – Basic set |  Option Set #2 – Additional
| -- | -- 
| - 100 - Continue |
| - 200 - OK | - 201 - Created 
|  | - 204 - No Content
| - 30x - Redirect | - 30x - Redirect 
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

Be explicit which 30x Redirect status codes the API supports. For any supported 30x codes follow the HTTP semantics.

### Principle #9 – Use of HTTP Header

Define all HTTP Headers that the API supports.

Use HTTP Headers as specified in [RFC 7231](https://tools.ietf.org/html/rfc7231), but design the API to allow overwriting of HTTP Headers based on URL query parameters.

For support of caching, consider supporting entity tags and the associated headers. However, their use might be in conflict when implementing security requirements. For these cases, you should explicitly name those headers that must be overwritten to avoid caching.

### Principle #10 - Allow flexible Content Negotiation

In HTTP, content negotiation is the mechanism that is used for serving different representations of a resource at the same URI, so that the user agent can specify which is best suited for the user (for example, which language of a document, which image format, or which content encoding). (Mozilla, 2021)

Content negotiation is an important aspect of any Web API specification and implementation and a special case of Principle #9.

The primary mechanisms for HTTP based content negotiation are these request headers:

* Accept: Which media types are acceptable for the response, such as "application/json," "application/xml," or a custom media type such as "application/vnd.example+xml"
* Accept-Charset: Which character sets are acceptable, such as UTF-8 or ISO 8859-1.
* Accept-Encoding: Which content encodings are acceptable, such as gzip.
* Accept-Language: The preferred natural language, such as "en-us".

In OGC Web API specification development, use an HTTP request header such as 'Accept' or 'Accept-Language' to request the response in a particular content type or language as defined in [RFC 7231](https://tools.ietf.org/html/rfc7231).

Use registered [IANA Media Types](https://www.iana.org/assignments/media-types/media-types.xhtml) whenever possible. 

An example for content negotiation based on HTTP headers and with query parameter override:

   HTTP 1.1 GET .../collections/highways/items/A8
   accept: application/geo+json
   => should return the response using the GeoJSON encoding

   HTTP 1.1 GET .../collections/highways/items/A8?accept=application%2Fgml%2Bxml
   accept: application/json
   => should return the response using the GML encoding


### Principle #11 - Pagination

OGC Web APIs that may be designed to access large data collections should support pagination. Offset pagination is one of the simplest to implement. Offset pagination is specified using the limit and offset commands. Offset pagination is popular with apps powered by SQL databases, as limit and offset are already included with the SQL SELECT library. Offset pagination requires almost no programming. It’s also stateless on the server side and works regardless of custom sort_by parameters.

The downside of offset pagination is potential performance difficulties when dealing with large offset values and non deterministic reads if resource creation, modification and deletion is allowed.

As an example, use **limit** and **offset** as "query-string" parameters.

    .../collections/highways/items?limit=50&offset=101 => returns upto 50 highways starting at position 101

The OGC defined Web API should return metadata with each response providing the total number of resources available (e.g. total) in the payload as well as the link to the next page.

As a supplement consider support for Web Linking (RFC 5988)

– Use HTTP Response Header to provide URLs for fetching the next / previous page
– This approach is application neutral and should be provided by the API as the default

### Principle #12 – Processing Resources

Use **verbs** to offer **operations** on resources. Verbs - not to be confused with HTTP methods - specify an action to be performed on a specific resource or a collection of resources. For example:

    .../transform => represents a processing resource that specifies transforming a resource

The parameters of the process are provided as query parameters, for example:

    .../transform?in=.../collections/highways/items/A8&toCRS=http://www.opengis.net/def/crs/EPSG/0/3258 => returns the A8 highway in the coordinate reference system ETRS89 lat/long

Note that the result of the example above may result in the same response as a selection/negotiation parameter on the resource (see Principle #6). For example:

    .../collections/highways/items/A8?crs=http://www.opengis.net/def/crs/EPSG/0/3258

An OGC API standard may allow offering processing resources as separate operations to support an explicit separation and highlight the processing capability. This allows publishing explicit metadata about the process, such as the input and output data structures.

### Principle #13 – Support Metadata

Providing metadata support as part of an OGC Web API specification helps the both the specification and implementation developer to understand how to use data or processing resources supported in the API specification. 

For example, one can associate metadata with a given resources though an association. Most notable, the `service-meta` and `data-meta` link relation types.

Regardless of the approach taken, use it consistently.
    

### Principle #14 – Consider your Security needs

Try to follow common practices for security in OGC Web APIs. For example:

- Provide access to the OGC API using HTTPS (RFC 2818).
- Include support for authentication from the beginning. 
- Consider consistent support for CRUD (Create, Read, Update, Delete) from the beginning (see Principle #5);
- Support for Execute may be provided on processing resources (see Principle #12) or using POST (see Principle #5).

### Principle #15 – API Description

Describing the OGC API in human and machine readable form has value to the developer. Currently the OGC is using OpenAPI version 3 as common practice for documenting an OGC API.

### Principle #16 - Use well-known identifiers

IANA has defined well-known identifiers for different purposes. For example:

- Media types: https://www.iana.org/assignments/media-types/media-types.xhtml
- Link relations: https://www.iana.org/assignments/link-relations/link-relations.xhtml
- Well-known URIs: https://www.iana.org/assignments/well-known-uris/well-known-uris.xhtml

For example is it possible to differenciate between XACML or GeoXACML policies. XACML policies would be returned with the 'application/xacml+xml' media type and GeoXACML policies with media type 'application/geoxacml+xml'.

For identifiers not registered with IANA, other standardization bodies like OGC should be used. For OGC for example, well-known identifiers can be registered with the OGC [Definition Server](https://www.ogc.org/def-server).

### Principle #17 - Use explicit relations
In many cases it is appropriate to use typed relation to explicitly declare links between resources. 

A special case are spatial relations between resources (e.g., topological relations such as: contains, within, etc.) which are easy to derive with a GIS, but not with Web clients unless the relations are explicitly represented. The relations may either be explicitly included in the resource representation or in Link headers in the HTTP response header (see RFC 5988).

### Principle #18 - Support W3C Cross-Origin Resource Sharing

If the OGC Web API is designed to be accessed by Web-applications executed in a Web Browser, support W3C CORS (https://www.w3.org/TR/cors/). 

Cross-Origin Resource Sharing (CORS) is an HTTP-header based mechanism that allows a server to indicate any other origins (domain, scheme, or port) than its own from which a browser should permit loading of resources. (Mozilla, 2021)

This approach provides the ability to overcome the security restrictions introduced by the Same-Origin Policy (https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) applied by the Web Browser, for example to JavaScript based applications when trying to access the Web API.

As identified in W3C CORS, in cross origin cases, the HTTP request carries specific HTTP headers and it is expected by the Web Browser that associated HTTP response headers exist in the response. Otherwise the processing stops. An example of a cross-origin request: the front-end JavaScript code served from https://domain-a.com uses XMLHttpRequest to make a request for https://domain-b.com/data.json.

### Principle #19 - Resource encodings

The API should provide resource representations based on the expectations of the implementers working in the domain the API is designed to support.

When designing an OGC API, a key decision is whether or not the Web API should support a default encoding that every implementation has to support. 

The SWG should recommend encodings fitting the resource type(s) both for machine (application) and human consumption.

For example, for machine reading of resources, JSON is - and XML was - commonly used. For human reading of resources or exploring capabilities of the API, HTML is commonly used.

### Principle #20 - Good APIs are testable from the beginning

Any OGC Web API developed according to these guidelines can already be tested during its prototyping phase. Considering all design principles including the identification of resource types, the effect of applying HTTP methods to them, the potential HTTP status codes, etc. provides the basis for documenting and implementing compliance tests in parallel with the API design and prototyping.

### Principle #21 - Specify whether operations are safe and/or idempotent 
For each operation one has to specify whether it has to be *safe* and/or *idempotent*. This is important, because clients and middelware rely on this.
This is helpful for identifying potential security issues when writing the security considerations of the Web API.

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

### Principle #22 – Make resources discoverable

To support [FAIR](https://www.go-fair.org/), discoverability is key (it is one option to address the F in FAIR). When a resource has other resources associated with it, make the associated resources discoverable.

For example through links in the document for that resource. Populate link elements and use link relations to choose appropriate "rel" properties for the links. When there are a large number of references and/or no link relation is appropriate, make the resource an array of other sub-resources. In these sub-recources of the array, include the most essential properties and provide a link with a self "rel" that points to the complete document. Another alternative is a URL template, see principle #4.

### Principle #23 - Make default behavior explicit

In case that the OGC Web API has a default behavior, it should be specified in a dominant place and be made explicit. A dominant place for the description of the default behavior could be the landing page, the response to a HTTP GET request with no parameters, the WEB API description document - e.g. OpenAPI specification, etc.

Good practice for defining default behavior is avoiding exception, error or empty pages as the response. 

Examples for default behavior:

 * response to a HTTP GET request if no accept headers or query parameters are specified in the request
 * the number of `items` returned (which might be controlled by a `limit` parameter (see principle #11)

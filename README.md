# OGC Web API Guidance

## A Comprehensive Set of Guidelines for developing OGC Web APIs 

Latest published version: 
 https://github.com/opengeospatial/OGC-Web-API-Guidelines/[*HTML*] 
 https://github.com/opengeospatial/OGC-Web-API-Guidelines.pdf[*PDF*]

## Background
The OGC has discussed new approches beyond the pure service oriented architecture (SOA) used in reveral successful services in the past and homogenized by OWS Common. The discussions suggested the convenence of a Resource Oriented Archtechture (ROA) that should focus on the concept of "resource" and the development of APIs on the web (Web APIs). In the past several of these discussions were around REST RESTful and hypermedia. After 10 years of discussions, the recommendation is to move on use the emerging Web API and take advantage of the concept of "resource" and ROA and avoid the term REST (even if some of the principles here are clearely inspired by REST and we are using Testbed 12 results about that). 

**Important note: This document is not addressing REST or RESTful but the broader concept or ROA Web APIs. the fact that a principle is not pure REST should not be an argument agaist a principle.**

## Starting point

[Follow the discussion on GitHub Issue 2 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/2)

The starting point of the Design Principles listed here was taken from a presentation [OGC Web API Design Principles](https://portal.opengeospatial.org/files/?artifact_id=78344) (requires OGC portal login) given during the OGC TC meetings in Orleans and Fort Collins. The presentation summerizes a collection of the Web API design principles used today by major players in main stream IT business. The purpose of the presentation is to ensure that the "common part of an API" is designed such that it can be re-used and a adopted easily. However, the initial presentation was not perfect in the sense that it might be incomplete and that there is room left for a good consensus discussion.

The original author of the presentation (Andreas Matheus), in collaboration with Charles Heazel, agreed to make the content available in this open Github repo for the purpose of creating a starting point in discussion and deriving a set of guidelines that could eventually be used to test OGC Web API Implementation Standards for conformance.

## Purpose and process

The implementation of Web APIs that allow the management of geospatial information should be possible by anyone familiar with the Web APIs designed for main stream IT. However, when designing a Web API by multiple domain experts, and not only by one team, and when trying to address multi purpose usage, it becomes challenging to ensure a common design pattern among all teams. 

To ensure that (i) Web API design across all different domains of expertise is coherent and (ii) the maximum from main stream IT design pattern is reused, the OGC Architecture Board (OAB) requested the elaboration of these guidelines. This process is done under the Architechture DWG with the colaboration of the OWS Common. For the moment this is a living document to inspire additional discussion and refinement within and among DWG and SWG teams, and contribute our learnings and suggestions to the technological community at large. The final aim is to reach consensus and converge in a document that the OGC can approve (possibly a white paper). 

With some of the desing patterns/principles published here, you can simply agree or disagree. For others, we require to select and conclude a particular way of doing things. For example, Design Principle #4 - CRUD requires that all different domain experts in the OGC agree to a common semantics what the effect of a particular method is when applied to a URI.

## Next steps

For the moment you should consider this to be a living, evolving document. Each principle in this document has a corresponding Issue on the GitHub site. Please use these Issues to discuss changes, corrections, and enhancements to the principles. 

Each Design Principle has a Common Objective that we like to conclude on. Please focus your feedback towards this objective. Once the discussions about a principle concludes on a stable outcome the issue will be closed and the principle frosen.

In the end the Architecture DWG would like to consider packaging the design principles as OGC Web API Design Guidelines White Paper. that will serve as a guidance the development of current and future OWS web services that adopt the Web API approach.

## Design Principles

### Why OGC API Design Principles 

The following is a set of common design principals for developing any Web API.

- The design of a Web API should follow a common pattern to ensure easy adoption.

- There are common design principles in main-stream IT that should be adopted to ease the adoption of OGC Web APIs.

- But still, there are some aspects that would need to be agreed upon to ensure seamless APIs for different thematic topics in OGC.

- In particular avoid that APIs are fundamentally different to access and manage different kinds of geospatial resources such as Features, Maps, Tiles, Coverages, Observations, Processes, etc.
    
Please follow the issues created for a discussion to observe the idea of the proposed OGC Web API design principles.  Please use these Issues to discuss changes, corrections, and enhancements to the principles.

**Important note: You may not and must not entirely agree with the presented principles. Important is to share your agreement or disagreement by contributing to an issue about the principle! Unfortunately issue numbers DOES NOT correspond to principle numbers. Relay on the issue title instead**

### Principle #1 – Don’t Reinvent

[Follow the discussion on GitHub Issue 3 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/3)

For aspects and functional capabilities that are already solved in main-stream IT and meet geospatial community requirement, simply adopt these API elements.

Focus instead on geo-centric and domain specific requirements to create new APIs or extend existing APIs. There is plenty of work to be done here.


#### OBJECTIVE: What is the best way to ensure re-use or adaption of existing design patterns?

### Principle #2 – Keep It Simple and Intuitive

[Follow the discussion on GitHub Issue 4 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/4)

Who are you targeting?

Know your audience!

What are you trying to achieve?

Make the developer of the API successful as quickly as possible!

Don’t forget to build in security from the start!

API design should not begin with technical documentation, but should rather originate from your fundamental goals.

#### OBJECTIVE: Ensure Web API design review in early stage. But how?

### Principle #3a - Use Well-Known Resource Types

[Follow the discussion on GitHub Issue 18 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/18)

Identify your resources (resource types)

Testbed 12 defined over 20 open geospatial Resource Types (below). Geospatial Enterprises should use these well-known definitions their APIs.

| Resource | Definition
| -- | -- 
| Capabilities | The complete service metadata document.
| Tile | A rectangular pictorial representation of geographic data, often part of a set of such elements, covering a spatially contiguous extent and sharing similar information content and graphical styling, which can be uniquely defined by a pair of indices for the column and row along with an identifier for the tile matrix.
| FeatureInfo | Insert definition here from Wiki when thereâs time.
| Schema | The complete application schema offered by the server.
| Feature Type | A feature type (i.e. a named collection of features with the same schema)
| Feature | A feature (i.e. a member of a feature type)
| Feature Type Property | A named property from the schema of a feature type.
| Feature Property | A named property from the schema of a feature.
| Query | A complex query resource.
| Transaction | A complex transaction resource.
| Process | Detailed process description of a single process.
| Process Collection | List of processes available.
| JobCollection | List of jobs of a process.
| Job | Representation of a job (execution of a process) containing status information.
| Process Output Data | Resource containing the different process outputs inline or as reference.
| Coverage | Full coverage in the format negotiated by the client and server through the proper HTTP Headers (e.g. Accept)
| Coverage Description | Full metadata regarding one specific coverage in negotiated format.
| Coverage Subset | A coverage derived on the fly from a subset operation applied to a persistent coverage. *The subset of a coverage is still a coverage.
| Coverage Range | A coverage derived on the fly from a range subsetting operation applied to a persistent coverage *The range subset of a coverage is still a coverage.

#### OBJECTIVE: Know the types or resources you have in preparation for defining a URL structure.

### Principle #3b – Keep the Base URL Simple

[Follow the discussion on GitHub Issue 5 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/5) and
[Follow the discussion on GitHub Issue 19 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/19)

Never release an API without versioning information

Use a version string as the most left most element of your path

    /v1 (don’t use dot notation)

Add the kind of resource to the path

    /v1/{features, maps, tiles, coverages, processes}

Use **nouns** not verbs to build the path to resources

    /v1/features/highway or /v1/maps/topo25 etc.

Use HTTP methods as verbs

    GET /v1/features/highway or PUT /v1/features/highway/A8
    
Great Web APIs look like they were designed by a single team. The most obvious properties of an API are the access paths and the URL templates which define them. Therefore, OGC conventions for the construction of access path templates are essential. Some of these templates are emerging though the Web Feature Service 3.0 efforts. They should be captured, formalized, and provided for re-use.

#### OBJECTIVE: Finalize the URL structure including the versioning aspect.

### Principle #4 – Use CRUD

[Follow the discussion on GitHub Issue 6 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/6)

Allow CRUD operations: **Create, Read, Update, Delete,** and **Execute**

Allow HTTP methods that operate on resources; **GET, POST, PUT, DELETE**

Define the semantics carefully when a method is invoked on a particular resource / resources determined by the path. E.g.

| Resource  | POST  | GET  | PUT | DELETE 
| -- | -- | -- | -- | -- 
| /.../highways | create a new highway | list all highways | bulk update of highways | delete all highways  
| /.../highways/A8 | Error! | show A8 | If exists: Update A8 else: Create A8 | delete highway A8

Do not force all semantics in just HTTP GET!

Also support HTTP communication methods:

    HEAD to return HTTP Headers with no payload
    OPTIONS to support W3C CORS

#### OBJECTIVE: Agree on the semantics of HTTP methods on a particular (resource) URL

### Principle #5 – Don’t mix Singular and Plural

[Follow the discussion on GitHub Issue 7 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/7)

It doesn’t matter if you use singular or plural for your nouns to build the path but, don’t mix them!

Use concrete names to build the path

    /v1/<kind>/<type>

kind using plural := {features, maps, tiles, coverages, observations, processes, facts, ...}

kind using singular := {feature, map, tile, coverage, observation, process, fact, …}

Examples of plural:

    POST /v1/features/tiger_roads/
    PUT /v1/maps/green_spaces/area51
    GET /v1/tiles/water_ways/4326/0/0

#### OBJECTIVE: Agree on the use of **singular** or **plural**

### Principle #6 – Put Complexity behind the ‘?’

[Follow the discussion on GitHub Issue 8 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/8)

This where the fun begins. The radical idea behind the '?' concept is that everything **left** of the **'?'** (the path design) should be identical regardless of the kind of resource and that everything **right** of the **'?'** may introduce domain specific aspects.

The "query-string" parameters should be used to select a resource(s) based on the(ir) characteristics - aka filter

    /v1/features/highways?id=A8 => returns highway A8
    /v1/features/highways?id=A8,A9 => returns highways A8 and A9

But what if you select on a resource instance and add a filter parameter?

    /v1/features/highways/A8?id=A8
    
Should the request above return *true* or *the resource itself*?

    /v1/features/highways/A8?id=A81

Should the request above return *false* or '*NULL*' (assuming the id of A8 is not A81)?

Use of the query string to select resources is highly domain specific and must be described on a case by case basis.

#### OBJECTIVE: Identify all the common elements that exist to the **right** of the **'?'**.

### Principle #7 – Error Handling

[Follow the discussion on GitHub Issue 9 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/9)

Error Codes are the developers insight into your API. So be precise and as detailed as possible.

Error handling is often one of the biggest complaints when using an API.

Usually, errors are associated with HTTP status codes.

However, support a "switch off" that always returns a status code 200 plus additional (debug / insight) information in the response body

    e.g. ?suppress_response_codes=true

Return detailed human readable error no. + description +
information on how to fix things + contact details

    { "developer_message": "…",
      "user_message": "...",
      "error_code": "...",
      "contact_details": "..."  
    }

#### OBJECTIVE: Agree on the mechanism and the level of detail.

### Principle #8 – HTTP Status Codes

[Follow the discussion on GitHub Issue 10 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/10)

More then 70 HTTP status codes exist (summary in RFC 7231).  You should reduce the use in your API to the most important ones. For example:

| Option Set #1 – Basic set |  Option Set #2 – Additional
| -- | -- 
| - 200 - OK | - 201 - Created 
|  | - 304 - Not Modified 
| - 400 - Bad Request | - 401 Unauthorized  
|  | - 403 - Forbidden 
|  | - 404 - Not Found 
|  | - 405 - Method Not Allowed 
|  | - 409 - Conflict 
|  | - 410 - Gone 
|  | - 412 - Precondition Failed 
| - 500 - Internal Server Error | - 503 - Service Unavailable 

But what to do with the 'redirect' status codes? To be used with care: 301, 302, 303, 307 and 308 or not allowed?

What about status code 100?

#### OBJECTIVE: Agree on a minimum set and their semantics.

### Principle #9 – Use of HTTP Header

[Follow the discussion on GitHub Issue 11 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/11)

The HTTP Header is used to provide information that *does not* change the logic of the API (e.g. HTTP Authorization)

Keep in mind that not all JS-API support the use of HTTP request headers! (e.g. Leaflet JS Library)

For Web-Applications use of “non-default” headers has performance pushback for cross-domain requests

    e.g. HTTP Authorization header on a cross-domain request causes execution of CORS (W3C Recommendation) => Pre-flight request…

#### OBJECTIVE: Agree upon which information is to be carried by HTTP headers.

### Principle #10 - Pagination

[Follow the discussion on GitHub Issue 12 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/12)

Use of **limit** and **offset** as "query-string" parameters is common sense

    e.g. /v1/features/highways?limit=50&offset=100

Issue: The API must return metadata with each response providing the total number of resources available (e.g. total) in the payload.

As an alternative to application processing of the response, you should try to use Web Linking (RFC 5988)

    – Use HTTP Response Header to provide URLs for fetching the next / previous page
    
    – This approach is application neutral and should be provided by the API as the default

#### OBJECTIVE: Agree on a common approach to be used.

### Principle #11 – Partial Responses

[Follow the discussion on GitHub Issue 13 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/13)

Allow the developer to fetch just not all of the detail of a resource but to also select the fields of interest.

How to design the API to support this?

Option 1: Integrate into path:

    /v1/features/highways/A8:(name, geometry)

Option 2: Use comma separated list as query parameter e.g. fields

    /v1/features/highways/A8?fields=name,geometry

The advantage of option 2 is that this approach can be combined with other search options:

    /v1/features/highways?id=A8&fields=name,geometry

#### OBJECTIVE: Agree on a common approach to be used.

### Principle #12 – Not Accessing Resources

[Follow the discussion on GitHub Issue 14 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/14)

Use **verbs** to offer **operations** on resources

    /v1/transform

An example is a request to transform a feature to a different CRS that the source:

    /v1/transform/features/highway/A8?
    fromCRS=4326&toCRS=WGS84

#### OBJECTIVE: Identify the **verbs** used across OGC domains.

### Principle #13 – Metadata

[Follow the discussion on GitHub Issue 15 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/15)

This part of the API helps the developer to understand what “the deal is with the resources”

Do not use a path pointing to resources. Use a clear and dedicated endpoint to provide metadata.

    clearly different from the resource endpoints
    /v1/metadata or /v1/discover

Starting with the next level, use URL path structures used for accessing resources

    /v1/metadata/features
    /v1/metadata/features/highways
    /v1/metadata/features/highways/A8
    
Allow the use of the ‘?’ operator to send selection queries (specify a search or filter option).

#### OBJECTIVE: Agree on the mechanism or conclude on an alternative approach.

### Principle #14 – Security

[Follow the discussion on GitHub Issue 16 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/16)

Host your API on HTTPS.

Require OAuth2 Bearer Tokens to control access. Use OpenID Connect to fetch user claims.

#### OBJECTIVE: Complete this principle based on your requirements.

### Principle #15 – API Description

[Follow the discussion on GitHub Issue 17 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/17)

Describing the API in OpenAPI has value to the developer.

#### OBJECTIVE: Agree on using OpenAPI (plus version) to describe the Web API.

### Principle #16 - Content-Type Negotiation

[Follow the discussion on GitHub Issue 18 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/18)

Use HTTP request header 'Content-Type' to request the response in a particular content type as defined in [RFC 1341](https://tools.ietf.org/html/rfc1341)

Use [IANA Media Types](https://www.iana.org/assignments/media-types/media-types.xhtml) whenever possible. 

#### OBJECTIVE: If using own content-types, have then register at IANA to ensure interoperability.

### Principle 17 - Use Well-Known URIs

[Follow the discussion on GitHub Issue 27 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/27)

IANA and other standardization organizations have defined so called well known URLs for different purposes.

E.g. [Well Known URIs](https://www.iana.org/assignments/well-known-uris/well-known-uris.xhtml)

#### OBJECTIVE: Identify if set of Well-Known URIs is complete (sufficient).

### Principle 18 - Good APIs are testable at Design Phase already

[Follow the discussion on GitHub Issue 20 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/20)

Any OGC Web API developed according to the developed guidelines can be tested at design phase already by validating compliance with the developed design principles. Possible results when assessing a principle could be (i) "the designed API is conformant with  principle #x", or (ii) "the desinged API is **not** conformant with principle #x. Reason: **...**"

Testbed 14 is developing a compliance test for WFS 3.0.  This compliance test starts with the OpenAPI (OAS) document for a WFS 3.0 service. It then traverses the OAD document looking for testable paths. This process requires that the service and OAS document comply with the conventions developed through the WFS 3.0 effort. These conventions will become refined and formalized over time.  An API which does not comply with these conventions will not be testable.  

#### OBJECTIVE: Test the Design Principles above by asserting your Web API and report compliance / divergence to the OGC Architecture DWG or OWS Common SWG.

### Principle 19 - Make use of geospatial relations

[Follow the discussion on GitHub Issue 23 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/23)

In many cases it is appropiate to use hypermedia and hyperlinks to explicitelly declare links among resources, BUT do not be ashame of making use of implicit geospatial relations. Georeference and co-location are intrinsically geospatial relations that make geospatial information unique. You can assume that both clients and services are geospatial aware and can make use of them. For example, given a cell (pixel) of a coverage, there is always a cell immediatelly to the north, another imediatelly to the south, east, and west. The same is true for 2D tiles. Make this relations explicity is possible but extremelly unefficient. A geospatial client (and server) should have this relations hardcoded and use them and there is no need to comunicate them. In some other cases explicit relations could be useful: it could be worth to state topological relations among polygons (e.g. contiguity) that otherwise could be time consuming to re-calculate.

#### OBJECTIVE: Accept that implicit geospatial relations are as good as explicit hypermedia relations in geospatial web services

### Principle 100 - Miscelleanous

[Follow the discussion on GitHub Issue 100 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/100)

Please report any missing priciple.

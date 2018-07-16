# OGC Web API Guidance

## A Comprehensive Set of Guidelines for developing OGC serivces using REST and OpenAPI.

Latest published version: 
 https://github.com/opengeospatial/OGC-Web-API-Guidelines/[*HTML*] 
 https://github.com/opengeospatial/OGC-Web-API-Guidelines.pdf[*PDF*]

## Purpose

Great RESTful APIs look like they were designed by a single team. This promotes API adoption, reduces friction, and enables clients to use them properly. To build APIs that meet this standard, and to answer many common questions encountered along the way of RESTful API development, the OGC Architecture Board (OAB) has created this set of guidelines. We have shared it with you to inspire additional discussion and refinement within and among your teams, and contribute our learnings and suggestions to the tech community at large.

## Usage

Feel free to use these guidelines as a guidance for your own development. You should consider this to be a living, evolving document. Each principle has a corresponding Issue on the GitHub site.  Please use these Issues to discuss changes, corrections, and enhancements to the principles..

## Design Principles

[Follow the discussion on GitHub Issue 2 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/2)

The Design Principles listed here are taken from a presentation [OGC Web Design Principles](https://portal.opengeospatial.org/files/?artifact_id=78344) (requires OGC portal login) given during the OGC TC meeting in Orleans and Fort Collins. The presentation summerizes a collection of the Web API design principles used today by major players in main stream IT business. The purpose of the presentation is to ensure that the "common part of an API" is designed such that it can be re-used and a adopted easily. However, the presentation is not perfect in the sense that it might be incomplete and that there is room left for a good consensus discussion.

The original author of the presentation has agreed to make the content available in this open Github repo for the purpose of creating a starting point in discussion and deriving a set of guidelines that could eventually be used to test OGC Web API Implementation Standards for conformance.

### Why API Design Principles 

The design of a Web API should follow a common pattern to ensure easy adoption.

There are common design principles in main-stream IT that should be adopted to ease the adoption of OGC Web APIs.

But still, there are some aspects that would need to be agreed upon to ensure seamless APIs for different thematic topics in OGC.

In particular avoid that APIs are fundamentally different to access and manage different kinds of geospatial assets such as Features, Maps, Tiles, Coverages, Observations, Processes, etc.
    
Please follow the issues created for a discussion to observe the idea of the proposed OGC Web API design principles.

**Important note: You may not and must not entirely agree with the presented principles. Important is to share your agreement or disagreement by contributing to an issue!**

### Principle #1 – Don’t Reinvent

[Follow the discussion on GitHub Issue 3 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/3)

Aspects that are already solved in main-stream IT, simply adopt.

Just focus on (OGC) domain specific aspects - And that will be a lot.

### Principle #2 – Keep It Simple and Intuitive

[Follow the discussion on GitHub Issue 4 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/4)

Who are you targeting?

What are you trying to achieve?

Make the developer of the API successful as quickly as possible!

Don’t forget to build in security from the start!

### Principle #3 – Keep the Base URL Simple

[Follow the discussion on GitHub Issue 5 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/5)

Never release an API without versioning

Use a version string as the most left of your path

    /v1 (don’t you dot notation)

Add the kind of resource to the path

    /v1/{features, maps, tiles, coverages, processes}

Use nouns not verbs to build the path

    /v1/features/highway or /v1/maps/topo25 etc.

Use two base URLs per resource kind

Use HTTP methods as verbs

    GET /v1/features/highway or /v1/features/highway/A8

### Principle #4 – Use CRUD

[Follow the discussion on GitHub Issue 6 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/6)

Allow CRUD **Create, Read, Update, Delete,** and **Execute**

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

### Principle #5 – Don’t mix Singular and Plural

[Follow the discussion on GitHub Issue 7 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/7)

It doesn’t matter if you use Singular or Plural for your nouns to build the path. But, don’t mix!

Use concrete names to build the path

    /v1/<kind>/<type>

kind using plural := {features, maps, tiles, coverages, observations, processes, facts, ...}

kind using singular := {feature, map, tile, coverage, observation, process, fact, …}

E.g. using plural

    POST /v1/features/tiger_roads/
    PUT /v1/maps/green_spaces/area51
    GET /v1/tiles/water_ways/4326/0/0

### Principle #6 – Put Complexity behind the ‘?’

[Follow the discussion on GitHub Issue 8 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/8)

This where the fun begins. The radical idea behind the '?' concept is that everything **left** of the **'?'** (the path design) should be identical regardless of the kind of resource and that everything **right** of the **'?'** may introduce domain specific aspects.

The query-string parameters should be used to select a resource(s) based on the(ir) characteristics - aka filter

    /v1/features/highways?id=A8 => returns highway A8
    /v1/features/highways?id=A8,A9 => returns highways A8 and A9

But what if you select on a resource instance and add a filter parameter?

    /v1/features/highways/A8?id=A8
    
should that return true or the resource?

    /v1/features/highways/A8?id=A81

should that return false or null (assuming the id of A8 is not A81)?

Use of the query string to select resources is highly domain specific and must be described on a case to case basis.

### Principle #7 – Error Handling

[Follow the discussion on GitHub Issue 9 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/9)

Error Codes are the developers insight into your API. So be precise and as detailed as possible.

Usually, errors are associated with HTTP status codes.

But, support a "switch off" which always returns a status code 200 plus additional (debug / insight) information in the response body

    e.g. ?suppress_response_codes=true

Return detailed human readable error no. + description +
information how to fix things + contact details

    { "developer_message": "…",
      "user_message": "...",
      "error_code": "...",
      "contact_details": "..."  
    }

### Principle #8 – HTTP Status Codes

[Follow the discussion on GitHub Issue 10 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/10)

More then 70 HTTP status codes exist (summary in RFC 7231).  You should reduce to the most important ones

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

### Principle #9 – Use of HTTP Header

[Follow the discussion on GitHub Issue 11 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/11)

HTTP Header are used to provide information that *does not* change the logic of the API (e.g. HTTP Authorization)

Keep in mind that not all JS-API support adding “stuff” into the HTTP request header! (e.g. Leaflet JS Library)

For Web-Applications use of “non-default” headers has performance pushback for cross-domain requests

    e.g. HTTP Authorization header on a cross-domain request causes execution of CORS (W3C Recommendation) => Pre-flight request…

### Principle #10 - Pagination

[Follow the discussion on GitHub Issue 12 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/12)

Use **limit** and **offset** as query string parameters is common sense

    e.g. /v1/features/highways?limit=50&offset=100

Issue: The API must return metadata with each response providing the total number of resources available (e.g. total) in the payload.

As an alternative to application processing the response, you should try to use Web Linking (RFC 5988)

    – Use HTTP Response Header to provide URLs for fetching the next / previous page
    
    – This approach is application neutral and should be provided by the API as the default

### Principle #11 – Partial Responses

[Follow the discussion on GitHub Issue 13 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/13)

Allow the developer to fetch just not all the detail of a resource but select the fields of interest

How to design the API to support this?

Option 1: Integrate into path

    /v1/features/highways/A8:(name, geometry)

Option 2: Use comma separated list as query parameter e.g. fields

    /v1/features/highways/A8?fields=name,geometry

Advantage of option 2: It can be combined with other search options

    /v1/features/highways?id=A8&fields=name,geometry

### Principle #12 – Not Accessing Resources

[Follow the discussion on GitHub Issue 14 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/14)

Use **verbs** to offer **operations** on resources

    /v1/transform

Example to transform feature using different CRS

    /v1/transform/features/highway/A8?
    fromCRS=4326&toCRS=WGS84

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
    
Allow ‘?’ operator to send selection queries (specify a search or filter option).

### Principle #14 – Security

[Follow the discussion on GitHub Issue 16 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/16)

Host your API on HTTPS.

. Require OAuth2 Bearer Tokens to control access
. Use OpenID Connect to fetch use claims

### Principle #15 – API Description

[Follow the discussion on GitHub Issue 17 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/17)

Describing the API in OpenAPI has value to the developer.

### Principle #16 - Content-Type Negotiation

[Follow the discussion on GitHub Issue 18 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/18)

Use HTTP request header 'Content-Type' to request the response in a particular content type as defined in [RFC 1341](https://tools.ietf.org/html/rfc1341)

Use [IANA Media Types](https://www.iana.org/assignments/media-types/media-types.xhtml) whenever possible. 

### Principle 17 - Use Well-Known URIs

[Follow the discussion on GitHub Issue 19 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/19)

IANA and other standardization organizations have defined so called well known URLs for different purposes.

E.g. [Well Known URIs](https://www.iana.org/assignments/well-known-uris/well-known-uris.xhtml)

### Principle 18 - Good APIs are testable at Design Phase already

[Follow the discussion on GitHub Issue 20 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/20)

Any OGC Web API developed according to the developed guidelines can be tested at design phase already by validating compliance with the developed design principles. Possible results when assessing a principle could be (i) "the designed API is conformant with  principle #x", or (ii) "the desinged API is **not** conformant with principle #x. Reason: **...**"


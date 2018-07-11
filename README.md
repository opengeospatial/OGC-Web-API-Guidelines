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

### Why API Design Principles 

The design of a Web API should follow a common pattern to ensure easy adoption

There are common design principles in main-stream IT that should be adopted to ease the adoption of OGC Web APIs

But still, there are some aspects that would need to be agreed upon to ensure seamless APIs for different thematic topics in OGC.

In particular avoid that APIs are fundamentally different to access and manage Features, Maps, Tiles, Coverages, Observations, Processes, etc.
    
Please follow this presentation to observe the idea of the proposed OGC Web API design principles.

### Principle #1 – Don’t Reinvent

[Follow the discussion on GitHub Issue 3 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/3)

Aspects that are already solved in main-stream IT, simply adopt.

Just domain specific aspects - And that will be a lot.

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

| Resource  | POST  | GET  | PUT | DELETE 
| -- | -- | -- | -- | -- 
| /.../highways | create a new highway | list all highways | bulk update of highways | delete all highways  
| /.../highways/A8 | Error! | show A8 | If exists: Update A8 else: Create A8 | delete highway A8

Also support HTTP communications methods:

    HEAD to return HTTP Headers with no payload
    OPTIONS to support CORS

### Principle #5 – Don’t mix Singular and Plural

[Follow the discussion on GitHub Issue 7 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/7)

It doesn’t matter if you use Singular or Plural for your
nouns to build the path. But, don’t mix!

Use concrete names to build the path

    /v1/<kind>/<type>

kinds using plural := {features, maps, tiles, coverages, observations, processes, facts, ...}

kinds using singular := {feature, map, tile, coverage, observation, process, fact, …}

E.g. using plural

    POST /v1/features/tiger_roads/
    PUT /v1/maps/green_spaces/area51
    GET /v1/tiles/water_ways/4326/0/0

### Principle #6 – Put Complexity behind the ‘?’

[Follow the discussion on GitHub Issue 8 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/8)

This where the fun begins

query-string parameters should be used to select a resource(s) based on the(ir) characteristics

    /v1/features/highways?id=A8 => returns highway A8
    /v1/features/highways?id=A8,A9 => returns highways A8 and A9

But what if you select on a resource instance?

    /v1/features/highways/A8?id=A8
    
should that return true or the resource?

    /v1/features/highways/A8?id=A81

should that return false or null?

Use of the query string to select resources is highly domain specific and must be described on a case to case basis

### Principle #7 – Error Handling

[Follow the discussion on GitHub Issue 9 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/9)

Error Codes are the developers insight into your API. So be precise and as detailed as possible

Usually, errors are associated with HTTP status codes

But, support a "switch off" which always returns a status code 200 plus additional information in the response body

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

### Principle #9 – Use of HTTP Header

[Follow the discussion on GitHub Issue 11 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/11)

Used to provide information that *does not* change the logic of the API (e.g. HTTP Authorization)

Keep in mind that not all JS-API support adding “stuff” into the HTTP request header! (e.g. Leaflet JS Library)

For Web-Applications use of “non-default” headers has performance pushback for cross-domain requests

    e.g. HTTP Authorization header on a cross-domain request causes execution of CORS (W3C Recommendation) => Pre-flight request…

### Principle #10 - Pagination

[Follow the discussion on GitHub Issue 12 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/12)

Use **limit** and **offset** as query string parameters is common sense

    e.g. /v1/features/highways?limit=50&offset=100

You must return metadata with each response providing the total number of resources available (e.g. total)

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

Use a clear and dedicated endpoint to provide metadata

    clearly different from the resource endpoints
    /v1/metadata or /v1/discover

Starting with the next level, use URL path structures used for accessing resources

    /v1/metadata/features
    /v1/metadata/features/highways
    /v1/metadata/features/highways/A8
    
Allow ‘?’ operator to send selection queries

### Principle #14 – Security

[Follow the discussion on GitHub Issue 16 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/16)

Host your API on HTTPS

. Require OAuth2 Bearer Tokens to control access
. Use OpenID Connect to fetch use claims

### Principle #15 – Miscellaneous

[Follow the discussion on GitHub Issue 17 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/17)

Describe the API in OpenAPI has value to the developer

### Principle #16 - Use Well-Known Resource Classes

[Follow the discussion on GitHub Issue 18 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/18)

Testbed 12 defined over 20 open geospatial Resource Classes (below). Geospatial Enterprises should use these well-known definitions their APIs.

| Resource | Definition
| -- | -- 
| Capabilities | The complete service metadata document.
| Tile | A rectangular pictorial representation of geographic data, often part of a set of such elements, covering a spatially contiguous extent and sharing similar information content and graphical styling, which can be uniquely defined by a pair of indices for the column and row along with an identifier for the tile matrix.
| FeatureInfo | Insert definition here from Wiki when there’s time.
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

### Principle 17 - Use Well-Known URL Templates and Access Paths

[Follow the discussion on GitHub Issue 19 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/19)

Great RESTful APIs look like they were designed by a single team. The most obvious properties of an API are the access paths and the URL templates which define them. Therefore, OGC conventions for the construction of access path templates are essential.  Some of these templates are emerging though the Web Feature Service 3.0 efforts. They should be captured, formalized, and provided for re-use.

### Principle 18 - Great APIs are testable

[Follow the discussion on GitHub Issue 20 ](https://github.com/opengeospatial/OGC-Web-API-Guidelines/issues/20)

Testbed 14 is developing a compliance test for WFS 3.0.  This compliance test starts with the OpenAPI (OAS) document for a WFS 3.0 service. It then traverses the OAD document looking for testable paths. This process requires that the service and OAS document comply with the conventions developed through the WFS 3.0 effort. These conventions will become refined and formalized over time.  An API which does not comply with these conventions will not be testable.     

